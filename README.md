# sirius-view


用法
===

代码
---

    //实例化 文件系统
    $filesystem=new Filesystem();
        
    //指定 待查找文件所在路径
    $paths=['views'];
    //指定 待查找文件的后缀
    $extensions=['blade.php','php','html'];
    //实例化 文件视图 查找器
    $finder=new FileViewFinder($filesystem,$paths,$extensions);
        
    //实例化 引擎解析器
    $engineResolver=new EngineResolver();
    //注册 各种 解析引擎
    //PHP 引擎
    $engineResolver->register('php',function(){
        return new PhpEngine();
    });
    //文件 引擎
    $engineResolver->register('file',function(){
        return new FileEngine();
    });
    //Blade 引擎
    $engineResolver->register('blade',function() use($filesystem){
        //指定 缓存路径
        $cachePath='cached/views';
        //实例化 Blade 编译器
        $bladeCompiler=new BladeCompiler($filesystem,$cachePath);
            
        return new CompilerEngine($bladeCompiler);
    });
    
    //实例化 事件分发器
    $events=new Dispatcher();
        
    //实例化 视图 工厂
    $factory=new Factory($engineResolver,$finder,$events);
