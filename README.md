# Captcha-Recognizer
异步图形验证码识别程序（封装集成了若快、打码兔、联众、云打码等人工打码平台）采用策略设计模式

## 主要特性

- 采用策略设计模式分离各个打码平台；
- 异步方式实现支持多并发识别；
- 识别完成后自动事件通知；
- 反射方式获取识别策略;
- 人工识别准确率高达99%，平均速度在2-6秒左右，经测试若快打码速度最快；

## 控制台示例代码
	public static void Main(string[] args)
        {
            var decoder = new Decoder(Platform.RuoKuai);
            //若此处不设置Account，程序直接读取在策略代码中设置的默认值
            //decoder.Account=new Account
            //{
            //    SoftId = 0, // 软件ID（此ID需要注册开发者账号才可获得）
            //    TypeId = 0, // 验证码类型（四位字符或其他类型的验证码，根据各平台设置不同值）
            //    SoftKey = "", //软件Key （此Key也需要注册开发者账号才可获得）
            //    UserName = "", //账号（此账号为打码平台的普通用户账号，开发者账号不能进行图片识别）
            //    Password = "" //密码
            //};
            decoder.OnStart += (s, e) =>
            {
                Console.WriteLine("验证码（"+e.FilePath+"）识别启动……");
            };
            decoder.OnCompleted += (s, e) =>
            {
                Console.WriteLine("验证码（" + e.FilePath + "）识别完成：" + e.Code + "，耗时：" + (e.Milliseconds/1000) + "秒");
            };
            decoder.OnError += (s, e) =>
            {
                Console.WriteLine("验证码识别出错：" + e.Exception.Message);
            };
            for (var i = 1; i <= 3; i++)
            {
                decoder.Decode("c:\\checkcode"+i+".png");
            }
            Console.ReadKey();
	}
	
	
![控制台运行示例](https://github.com/coldicelion/Captcha-Recognizer/blob/master/Wesley.Component.Captcha.Example/Resources/running.jpg?raw=true)