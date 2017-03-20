config文件中未配置值

{
  "Message": "出现错误。",
  "ExceptionMessage": "尝试创建“ProductDetailsController”类型的控制器时出错。请确保控制器具有无参数公共构造函数。",
  "ExceptionType": "System.InvalidOperationException",
  "StackTrace": "   在 System.Web.Http.Dispatcher.DefaultHttpControllerActivator.Create(HttpRequestMessage request, HttpControllerDescriptor controllerDescriptor, Type controllerType)\r\n   在 System.Web.Http.Controllers.HttpControllerDescriptor.CreateController(HttpRequestMessage request)\r\n   在 System.Web.Http.Dispatcher.HttpControllerDispatcher.<SendAsync>d__1.MoveNext()",
  "InnerException": {
    "Message": "出现错误。",
    "ExceptionMessage": "Resolution of the dependency failed, type = \"Api.Product.Service.Controllers.ProductDetailsController\", name = \"(none)\".\r\nException occurred while: while resolving.\r\nException is: InvalidOperationException - The type String cannot be constructed. You must configure the container to supply this value.\r\n-----------------------------------------------\r\nAt the time of the exception, the container was:\r\n\r\n  Resolving Api.Product.Service.Controllers.ProductDetailsController,(none)\r\n  Resolving value for property ProductDetailsController.ProductBll\r\n    Resolving Api.Business.ScmGoods.BLLProductDetail,(none)\r\n    Resolving value for property BLLProductDetail.ScmApiDomain\r\n      Resolving System.String,AppSettings.ScmApiDomain\r\n",
    "ExceptionType": "Microsoft.Practices.Unity.ResolutionFailedException",
    "StackTrace": "   在 Microsoft.Practices.Unity.UnityContainer.DoBuildUp(Type t, Object existing, String name, IEnumerable`1 resolverOverrides)\r\n   在 Microsoft.Practices.Unity.UnityContainer.DoBuildUp(Type t, String name, IEnumerable`1 resolverOverrides)\r\n   在 Microsoft.Practices.Unity.UnityContainer.Resolve(Type t, String name, ResolverOverride[] resolverOverrides)\r\n   在 Microsoft.Practices.Unity.UnityContainerExtensions.Resolve(IUnityContainer container, Type t, ResolverOverride[] overrides)\r\n   在 Microsoft.Practices.Unity.WebApi.UnityDependencyResolver.SharedDependencyScope.GetService(Type serviceType)\r\n   在 System.Web.Http.Dispatcher.DefaultHttpControllerActivator.GetInstanceOrActivator(HttpRequestMessage request, Type controllerType, Func`1& activator)\r\n   在 System.Web.Http.Dispatcher.DefaultHttpControllerActivator.Create(HttpRequestMessage request, HttpControllerDescriptor controllerDescriptor, Type controllerType)",
    "InnerException": {
      "Message": "出现错误。",
      "ExceptionMessage": "The type String cannot be constructed. You must configure the container to supply this value.",
      "ExceptionType": "System.InvalidOperationException",
      "StackTrace": "   在 Microsoft.Practices.ObjectBuilder2.DynamicMethodConstructorStrategy.GuardTypeIsNonPrimitive(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicMethodConstructorStrategy.PreBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.StrategyChain.ExecuteBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicMethodBuildPlanCreatorPolicy.CreatePlan(IBuilderContext context, NamedTypeBuildKey buildKey)\r\n   在 Microsoft.Practices.ObjectBuilder2.BuildPlanStrategy.PreBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.StrategyChain.ExecuteBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.BuilderContext.NewBuildUp(NamedTypeBuildKey newBuildKey)\r\n   在 Microsoft.Practices.Unity.ObjectBuilder.NamedTypeDependencyResolverPolicy.Resolve(IBuilderContext context)\r\n   在 lambda_method(Closure , IBuilderContext )\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicBuildPlanGenerationContext.<>c__DisplayClass1.<GetBuildMethod>b__0(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicMethodBuildPlan.BuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.BuildPlanStrategy.PreBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.StrategyChain.ExecuteBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.BuilderContext.NewBuildUp(NamedTypeBuildKey newBuildKey)\r\n   在 Microsoft.Practices.Unity.ObjectBuilder.NamedTypeDependencyResolverPolicy.Resolve(IBuilderContext context)\r\n   在 lambda_method(Closure , IBuilderContext )\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicBuildPlanGenerationContext.<>c__DisplayClass1.<GetBuildMethod>b__0(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.DynamicMethodBuildPlan.BuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.BuildPlanStrategy.PreBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.ObjectBuilder2.StrategyChain.ExecuteBuildUp(IBuilderContext context)\r\n   在 Microsoft.Practices.Unity.UnityContainer.DoBuildUp(Type t, Object existing, String name, IEnumerable`1 resolverOverrides)"
    }
  }
}

分析1：
The type String cannot be constructed. You must configure the container to supply this value
<font color=red>指必须配置unity使用的内容</font>

分析2：
You must configure the container to supply this value.
At the time of the exception, the container was:
Resolving Api.Product.Service.Controllers.ProductDetailsController,(none)
Resolving value for property ProductDetailsController.ProductBll
Resolving Api.Business.ScmGoods.BLLProductDetail,(none)
Resolving value for property BLLProductDetail.ScmApiDomain
Resolving System.String,AppSettings.ScmApiDomain
<font color=red>指具体需要提供值的代码位置，根据提示一层层的找出未错误的地址</font>


分析3：
结论及实际原因：
<font color=red>
BLLProductDetail 类中使用了
[Dependency("AppSettings.ScmApiDomain")]
public string ScmApiDomain { get; set; }
但config中未配置ScmApiDomain，导致整个程序出错
</font>


