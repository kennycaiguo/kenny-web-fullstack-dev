# kenny-web-fullstack-dev
学习web全栈开发
课程修改源码下载地址: https://url90.ctfile.com/f/52133290-1024965940-ba02bc?p=9506   (访问密码: 9506)

## 注意:使用axios访问https网站会有一个证书过期的问题,解决方法是,实例化一个axios的示例,设置httpsAgent如下,
import express from "express"
import axios from "axios"
import https from "https"

//解决证书没有效的问题<br>
const instance = axios.create({  <br>
    httpsAgent: new https.Agent({  <br>
      rejectUnauthorized: false  <br>
    })  <br>
}); <br>
## 然后用示例来代替axios发送请求<br>
// 2. Create an express app and set the port number. <br>
let app = express() <br>
let port = 4000 <br>
// 3. Use the public folder for static files. <br>
app.use(express.static('public')) <br>
// 4. When the user goes to the home page it should render the index.ejs file. <br>
// 5. Use axios to get a random secret and pass it to index.ejs to display the <br>
// secret and the username of the secret. <br>
app.get("/",async (req,res)=>{ <br>
    try { <br>
        const result = await instance.get("https://secrets-api.appbrewery.com/random"); <br>
        res.render("index.ejs", { <br>
          secret: result.data.secret, <br>
          user: result.data.username, <br>
        });<br>
      } catch (error) { <br>
        console.log(error); //error.response.data这里会报错,是get请求方法的问题 <br>
        res.status(500); <br>
      }
})

// 6. Listen on your predefined port and start the server. <br>
app.listen(port,()=>{ <br>
    console.log(`server ready: http://localhost:${port}/`); <br>
}) <br>
