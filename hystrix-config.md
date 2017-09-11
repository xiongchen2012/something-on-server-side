
## Hystrix配置
---
### 配置的优先级

分为**全局**和**实例**两个优先级，两级分别有**默认值**和**动态值**两个优先级，故有4种优先级    
- 全局默认设置（Global default from code）
- 全局动态默认属性（Dynamic global default property）
- 实例默认设置（Instance default from code）
- 实例动态属性（Dynamic instance property）    


**优先级**：    
全局默认设置 < 全局动态默认属性 < 实例默认设置 < 实例动态属性    

### 配置方法

  - 全局默认设置    
  优先级最低。无需写任何代码进行配置。    
  Hystrix的默认值，当配置项没有其它3种配置时，此配置生效。

  - 全局动态默认属性    
  优先级仅高于全局默认设置，需要写代码进行设置。    
  当配置项没有实例的配置时，此配置能够生效。配置的方法：    

    ``` java    
    ConfigurationManager.getConfigInstance().setProperty(key,value);
    ```

    其中的key是指Hystrix的配置项名称，value为它的值。    
    全局动态默认属性的配置项名称一般是：hystrix.command.default.~~execution.isolation.strategy~~    
    hystrix.command.default：表示这是全局的默认配置项    
    execution.isolation.strategy：表示具体配置项的名称    

  - 实例默认设置
    实例的配置项优先级总是高于全局配置，实例默认设置作用于整个实例。    
    设置方法：通过HystrixCommand的构造函数，传递设置值。

    ``` java    
    HystrixCommandProperties.Setter()
       .withExecutionTimeoutInMilliseconds(int value)

    //通过调用HystrixCommand的构造函数，传递新的值
    super(Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("ExampleGroup"))
        .andCommandPropertiesDefaults(HystrixCommandProperties.Setter()
               .withExecutionTimeoutInMilliseconds(500)));     
    ```

  - 实例动态属性
    优先级最高，作用到CommandGroup中。    
    设置方法和全局动态默认属性相同，只将是key中的***default***换成具体的GroupKey就行了。如下：    

    ``` java    
     ConfigurationManager.getConfigInstance().setProperty(key,value);
    ```
    其中的key与全局动态默认属性不一样，全局的是：     
    ``` properties
      hystrix.command.default.execution.isolation.strategy    
    ```
    但是实例动态属性，需要把**default**替换为Key名，如：    
    ``` properties
      hystrix.command.GreenTravelScore.execution.isolation.strategy    
    ```
