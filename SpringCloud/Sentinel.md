## 限流（限流）
>接口限流保护
 + 接口作为资源进行保护  
>可以有哪些规则？  
 + 针对来源
 + QPS：设置阈值（10），表示每秒只可以放10个请求。
 + 并发线程数：
 + 是否集群（3个node，阈值100）  
   + 集群阈值模式--单机均摊、总体阈值
   + 单机均摊：
     + 每个机器都不会超过100
   + 总体阈值：
     + 三个服务整体是100，具体每个服务负载多少是根据负载均衡控制
> 流控模式
 + 直接：直接限制设定的服务（比如秒杀ms-seckill服务
 + 关联：
  ![](../pictures/%E9%99%90%E6%B5%81-%E5%85%B3%E8%81%94.png)
  + A写入数据，B读数据，其中A关联B资源，如果对A进行了限流（阈值为100），如果B的流量大，则会限制A流量
 + 链路：对C进行限流
  ![](../pictures/%E9%93%BE%E8%B7%AF%E8%B0%83%E7%94%A8.png)
  + 入口资源：为root，则从root->C调用则会进行限流。如果入口微F，则不会
>流控效果
 + 快速失败：直接抛出异常
 + WarmUp：预热/冷启动，峰值假设为200的阈值，假设有210个流量，则会通过设置的【预热时长-10s】慢慢的（10秒内）达到200。
 + 排队等待：如果来了300个流量，其中200则直接进行放行，剩下100进入队列，并可以设置等待【超时时间】

>自定义限制规则
 + 构建自定义的配置类，采用 WebCallbackManager.setUrlBlockHandler方法

## 熔断 & 降级
>在服务调用的过程中，如果某个服务出现了异常，则通过【熔断】机制使得调用方能够继续响应，从而避免异常等待。
>某个服务性能太差，则可以通过【降级】策略直接响应

## 自定义受保护的资源
1. 基于代码进行资源保护
```java
try(Entry entry = SphU.entry("seckillSkus")){
    // seckillSkus为限流资源的名称，在sentinel中进行配置的时候，需要填写该值
    // 需要被保护的代码

}catch(BlockExcepton e){
    // 这里写被限制后的输出
}
```
2. 基于注解进行资源保护
>在需要被保护的方法上增加注解：@SentinelResource
```java
@SentinelResource(value = "getCurrentSeckillSkus", blockHandler = "blockHandler", fallback = )
public List<SeckillSkuRedisTo> getCurrentSeckillSkus() {}

public List<SeckillSkuRedisTo> blockHandler() {
    //执行被限流/熔断/降级等规则后的业务处理
}

```
 + value对应的资源名，在sentinel中设置时用到
 + blockHandler限流后被执行的方法，需要与被限制的方法返回值、参加一致
 + fallback是针对异常后的统一处理，返回值、参数与原函数一致