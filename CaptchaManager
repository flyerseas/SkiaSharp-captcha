/// <summary>
    /// 验证码生成及管理
    /// </summary>
    public static class CaptchaManager
    {
        /// <summary>
        /// 生成验证码
        /// </summary>
        /// <param name="length"></param>
        /// <returns></returns>
        public static string GenerateCode(int length)
        {
            string str = "1,2,3,4,5,6,7,8,9,0,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z";
            string[] strArr = str.Split(',');
            StringBuilder code = new StringBuilder();
            for (int i = 0; i < length; i++)
            {
                code.Append(strArr[Random.Shared.Next(0, strArr.Length)]);
            }
            return code.ToString();
        }

        /// <summary>
        /// 生成图片流
        /// </summary>
        /// <param name="verifyCode"></param>
        /// <returns></returns>
        public static byte[] GenerateCaptcha(string verifyCode)
        {
            int height = 30;
            int width = 85;
            //创建bitmap位图
            using (SKBitmap image2d = new SKBitmap(width, height, SKColorType.Bgra8888, SKAlphaType.Premul))
            {
                //创建画笔
                using (SKCanvas canvas = new SKCanvas(image2d))
                {

                    canvas.Clear(SKColors.White);

                    // 在图片上写验证码
                    SKPaint textPaint = new SKPaint
                    {
                        // Typeface = GetTypeface(),
                        IsAntialias = true,
                        TextAlign = SKTextAlign.Left,
                        Shader = SKShader.CreateLinearGradient(new SKPoint(0, 0), new SKPoint(width, height), new SKColor[] { SKColors.Blue, SKColors.DarkRed }, SKShaderTileMode.Clamp),
                        TextSize = 22
                    };
                    canvas.DrawText(verifyCode, 4, 24, textPaint);

                    var ran = Random.Shared;

                    // 在图片上随机画5条线
                    for (int i = 0; i < 5; i++)
                    {
                        canvas.DrawLine(
                            new SKPoint(ran.Next(width), ran.Next(height)),
                            new SKPoint(ran.Next(width), ran.Next(height)),
                            new SKPaint() { Color = SKColor.FromHsv(ran.Next(0, 256), ran.Next(0, 256), ran.Next(0, 256)) }
                        );
                    }

                    // 在图片上随机画100个随机颜色点
                    for (int i = 0; i < 100; i++)
                    {
                        canvas.DrawPoint(new SKPoint(ran.Next(width), ran.Next(height)), SKColor.FromHsv(ran.Next(0, 256), ran.Next(0, 256), ran.Next(0, 256)));
                    }

                    using (SKData p = image2d.Encode(SKEncodedImageFormat.Png, 100))
                    {
                        return p.ToArray();
                    }
                }
            }
        }
    }

    /// <summary>
    /// 验证码签名对象
    /// </summary>
    public class CaptchSign
    {
        /// <summary>
        /// 验证码表示
        /// </summary>
        /// <value></value>
        [JsonPropertyName("identity")]
        public string Identity { get; set; }

        /// <summary>
        /// 加密后的签 MD5(identity + code)
        /// </summary>
        /// <value></value>
        [JsonPropertyName("sign")]
        public string Sign { get; set; }

        /// <summary>
        /// 验证码图片 base64编码
        /// </summary>
        /// <value></value>
        [JsonPropertyName("captcha")]
        public string Captcha { get; set; }

        /// <summary>
        /// 构造函数
        /// </summary>
        /// <param name="captcha">验证码</param>
        public CaptchSign(string captcha)
        {
            Identity = Guid.NewGuid().ToString("N");
            //生成验证码图片
            var bytes = CaptchaManager.GenerateCaptcha(string.Join(" ", captcha.ToList()));
            Captcha = Convert.ToBase64String(bytes);
            //签名
            this.Sign = Cryptography.MD5(Identity + captcha.ToLower()).ToLower();
        }
    }
