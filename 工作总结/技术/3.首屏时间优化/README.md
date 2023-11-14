# 治理流程
1. 制定RoadMap
2. 根据raptor平台数据，整理页面PV及其首评时间，确定治理页面
3. 分析各页面资源加载及代码情况
4. 梳理形成各页面治理方案并实践
5. 验证结果
# 优化方法
## 网络
### 相同接口做合并请求
对并行请求且url相同的接口，做请求参数的合并（例如获取远程字典的接口和功能权限的鉴权接口）
- 减少HTTP请求次数，减轻服务器压力，提高用户体验
- 提高数据传输效率
### 接口做缓存
对组件内部获取数据源的接口做缓存（添加{cache: true}参数）
- 一个页面中多次引用组件A，不同地方只是对源数据进行过滤展示等操作，添加cache后可以让组件内部的接口只请求一次
```
import { isEqual, pick } from 'lodash';
import { get, post } from './request';

// 接口缓存结果
const CachePromiseList: any[] = [];

function matchRequest(method: any, { prefix = '' } = {}, addOrigin = false) {
    return function (url: any, param: any, options = {}) {
        const { cache = false, useDefaultErrorCallback = true, ...restOptions } = options as any;
        let req = url;

        if (typeof url === 'string') {
            req = {};
            req.url = addOrigin ? [window.location.origin, url].join('') : url;
            req.options = restOptions;
            req[method === get ? 'params' : 'body'] = param;
        }
        if (req.url.startsWith('/')) {
            req.url = `${prefix}${req.url}`;
        }

        let rsPromise;

        // 是否使用缓存
        if (cache) {
            // 比较的参照物
            const reference = {
                url: req.url,
                method: method === get ? 'get' : 'post',
                params: req.params,
                body: req.body,
            };
            const itemCache = CachePromiseList.find((v) => (
                isEqual(pick(v, Object.keys(reference)), { ...reference })
            ));
            if (itemCache) {
                rsPromise = itemCache.rsPromise;
            } else {
                rsPromise = method(req);
                CachePromiseList.push({ ...reference, rsPromise });
            }
        } else {
            rsPromise = method(req);
        }

        // 请求成功时统一处理
        if ((window as any).__requestSuccCallback) {
            rsPromise.then((rs: any) => {
                if ((window as any).__requestSuccCallback) {
                    (window as any).__requestSuccCallback(rs, req);
                }
            });
        }
        // 请求失败时统一处理
        if (useDefaultErrorCallback && (window as any).__requestErrorCallback) {
            rsPromise.catch((e: any) => {
                if ((window as any).__requestErrorCallback) {
                    (window as any).__requestErrorCallback(e, req);
                }
            });
        }
        return rsPromise;
    };
}

export default (args?: any) => ({
    get: matchRequest(get, args),
    post: matchRequest(post, args),
    hGet: matchRequest(get, args, true),
    hPost: matchRequest(post, args, true),
    go(url: any) {
        if (url.startsWith('/')) {
            url = `${((args && args.prefix) || '')}${url}`;
        }
        window.location.href = url;
    },
});
```
### 
