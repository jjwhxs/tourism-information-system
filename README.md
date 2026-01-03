### 系统介绍

基于SpringBoot和Vue实现的旅游信息系统采用前后端分离的架构方式，系统设计了两种角色，分别是用户、管理员，每种角色拥有不同的菜单权限，用户可以在系统前台中进行注册、登录、首页、景点、攻略、周边住宿、门票预订、个人中心、我的攻略、我的收藏、我的订单等功能模块的操作，管理员可以在系统后台中进行登录、首页、用户管理、个人信息、景点管理、分类管理、评论管理、攻略管理、收藏管理、门票管理、订单管理、住宿管理、轮播图管理等功能模块的操作。

### 技术选型

开发工具：idea2022.3+webstorm2021.1

运行环境：jdk1.8+maven3.6.3+mysql8.0+nodejs16.0.0+redis5.0.9

服务端技术：springboot+mybatis-plus+jwt+fastjson

前端技术：html+css+vue+axios+element-ui+echarts

### 成果展示

系统登录/注册
<img width="1903" height="1021" alt="登录" src="https://github.com/user-attachments/assets/4f3419ae-b3da-4124-a710-b43aa048d960" />

前台系统->首页
<img width="1910" height="2801" alt="首页" src="https://github.com/user-attachments/assets/215ba5db-01ad-42a3-ad70-65f2505c378a" />

前台系统->景点
<img width="1910" height="2165" alt="景点" src="https://github.com/user-attachments/assets/01fe246e-086e-48bb-b168-b4428c42fbbd" />

前台系统->攻略
<img width="1884" height="1025" alt="攻略" src="https://github.com/user-attachments/assets/dd44d7cf-f9f8-455b-92ce-120e2bae352b" />

前台系统->周边住宿
<img width="1910" height="3320" alt="周边住宿" src="https://github.com/user-attachments/assets/2848f0b4-41fb-4224-a514-2b14b7b578fa" />

前台系统->门票预订
<img width="1910" height="2425" alt="门票预订" src="https://github.com/user-attachments/assets/41cd67d2-6f8f-4276-8ed1-f030af23f046" />

前台系统->我的订单
<img width="1883" height="1021" alt="我的订单" src="https://github.com/user-attachments/assets/8eb47c01-06c2-400b-bbda-9568dc5813dd" />

后台系统->首页
<img width="1900" height="1025" alt="后台系统-首页" src="https://github.com/user-attachments/assets/b42c393b-0eb7-4f1a-98a6-897413384c1f" />

后台系统->用户管理
<img width="1894" height="1025" alt="后台系统-用户管理" src="https://github.com/user-attachments/assets/3177afda-7fb3-4b57-be79-daac002f8914" />

后台系统->分类管理
<img width="1901" height="1025" alt="后台系统-分类管理" src="https://github.com/user-attachments/assets/9d7ec6f6-8258-4742-bd89-8c9f46e2c88a" />

后台系统->评论管理
<img width="1893" height="1019" alt="后台系统-评论管理" src="https://github.com/user-attachments/assets/a0559b46-f561-4b24-97a7-f955a229b913" />

后台系统->攻略管理
<img width="1894" height="1025" alt="后台系统-攻略管理" src="https://github.com/user-attachments/assets/8c64a701-f7b7-4b77-a1cb-9cd6c1e56dcd" />

后台系统->收藏管理
<img width="1896" height="1025" alt="后台系统-收藏管理" src="https://github.com/user-attachments/assets/15a67a69-3405-4a79-ac20-0e9196a7336d" />

后台系统->门票管理
<img width="1895" height="1025" alt="后台系统-门票管理" src="https://github.com/user-attachments/assets/79098c8d-74d3-471a-bdfd-fa8e436d8e35" />

### 源码展示

@Tag(name="门票订单管理接口")

@RestController

@RequestMapping("/order")

public class TicketOrderController {

private static final Logger LOGGER = LoggerFactory.getLogger(TicketOrderController.class);

@Resource

private TicketOrderService ticketOrderService;

@Operation(summary = "创建门票订单")

@PostMapping

public Result<?> createOrder(@RequestBody TicketOrder order) {

    TicketOrder createdOrder = ticketOrderService.createOrder(order);
    return Result.success(createdOrder);
    
}

@Operation(summary = "支付订单")

@PostMapping("/{id}/pay")

public Result<?> payOrder(@PathVariable Long id, @RequestParam String paymentMethod) {

    ticketOrderService.payOrder(id, paymentMethod);
    return Result.success();
    
}

@Operation(summary = "取消订单")

@PostMapping("/{id}/cancel")

public Result<?> cancelOrder(@PathVariable Long id) {

    ticketOrderService.cancelOrder(id);
    return Result.success();
    
}

@Operation(summary = "退款订单（管理员专用）")

@PostMapping("/{id}/refund")

public Result<?> refundOrder(@PathVariable Long id) {

    ticketOrderService.refundOrder(id);
    return Result.success();
    
}

@Operation(summary = "完成订单（管理员专用）")

@PostMapping("/{id}/complete")

public Result<?> completeOrder(@PathVariable Long id) {

    ticketOrderService.completeOrder(id);
    return Result.success();
    
}

@Operation(summary = "获取当前用户订单列表")

@GetMapping("/my")

public Result<?> getMyOrders(@RequestParam(required = false) Integer status, @RequestParam(defaultValue = "1") Integer currentPage, @RequestParam(defaultValue = "10") Integer size) {

    User currentUser = JwtTokenUtils.getCurrentUser();
    if (currentUser == null) {
        return Result.error("用户未登录");
    }
    
    Page<TicketOrder> page = ticketOrderService.getUserOrders(currentUser.getId(), status, currentPage, size);
    return Result.success(page);
    
}

@Operation(summary = "获取订单详情")

@GetMapping("/{id}")

public Result<?> getOrderDetail(@PathVariable Long id) {

    TicketOrder order = ticketOrderService.getOrderDetail(id);
    return Result.success(order);
    
}

@Operation(summary = "根据订单号查询订单状态")

@GetMapping("/status")

public Result<?> getOrderStatus(@RequestParam String orderNo) {

    TicketOrder order = ticketOrderService.getOrderByOrderNo(orderNo);
    if (order == null) {
        return Result.error("订单不存在");
    }
    return Result.success(order);
    
}

@Operation(summary = "获取订单统计信息")

@GetMapping("/stats")

public Result<?> getOrderStats() {

    // 验证当前用户是否是管理员
    User currentUser = JwtTokenUtils.getCurrentUser();
    if (currentUser == null || !"ADMIN".equals(currentUser.getRoleCode())) {
        return Result.error("无权访问");
    }
    
    // 这里可以实现订单统计逻辑，比如按状态统计数量、总金额等
    // 这里简单返回一个示例数据
    Map<String, Object> stats = new HashMap<>();
    stats.put("totalOrders", 100);
    stats.put("totalAmount", 10000);
    stats.put("pendingPayment", 20);
    stats.put("paid", 70);
    stats.put("cancelled", 8);
    stats.put("refunded", 2);
    stats.put("completed", 0);
    
    return Result.success(stats);
    
}

@Operation(summary = "删除订单")

@DeleteMapping("/{id}")

public Result<?> deleteOrder(@PathVariable Long id) {

    ticketOrderService.deleteOrder(id);
    return Result.success("订单删除成功");
    
}

}

### 账号地址及其它说明

1、地址说明

登录页：localhost:8080

2、账号说明

用户：zhangsan/123456

管理员：admin/123456

3、目录结构展示

<img width="715" height="175" alt="目录结构" src="https://github.com/user-attachments/assets/123cee72-00ed-418f-bb28-aa73519b5ebb" />

4、项目结构展示

<img width="1546" height="831" alt="项目结构" src="https://github.com/user-attachments/assets/1ef75eef-9cb0-4425-8a4c-3abfd4ceda70" />

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.properties中的数据库配置文件，启动服务端

3）在前端工程根目录下打开cmd，执行npm install下载依赖

4）下载完毕后启动前端npm run serve，访问端口

### 获取方式(可远程调试)

访问链接(在浏览器中手动输入下图中的地址)：

<img width="1129" height="124" alt="链接" src="https://github.com/user-attachments/assets/08a0244e-5d5d-4997-b374-de677198094a" />

若资源获取失败，可添加happy35596339(vx)或2061772307(qq)进行交流
