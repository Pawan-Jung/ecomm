const http = require('http');
const fs = require('fs');
const url = require('url');
const querystring = require('querystring');
const server = http.createServer((req,res)=>{
    let productsdata = [];
    let qs = url.parse(req.url);
    const params = querystring.parse(qs.query)
    const pt = `/?category=${params.category}`;
    if(req.url == '/')
    {
        const productPath = __dirname + '/products.json';
        fs.readFile( productPath, 'utf-8' ,(err,data)=>{
            if(err)
            {
                res.writeHead(400 , {'Content-Type':'text/plain'});
                res.end('err');
            }
            else 
            {
                res.writeHead(200 , {'Content-Type':'text/plain'});
                productsdata.push(data);
                res.end(data);
            }
        })
    }
    else  if(req.url == pt)
    {
        const productPath = __dirname + '/products.json';
        let userDataShow = [];
        fs.readFile( productPath, 'utf-8' ,(err,data)=>{
            if(err)
            {
                res.writeHead(400 , {'Content-Type':'text/plain'});
                res.end('err');
            }
            else 
            {
               data = JSON.parse(data);
               data.forEach(element => {
                if(element.category == params.category)
                {
                    userDataShow.push(element);
                }
               });
               res.end(JSON.stringify(userDataShow));
            }
        })
    }
    else 
    {
        res.writeHead(403 , {'Content-Type':'text/plain'});
        res.end(`Page Not found`);
    }
    // res.end('hello chandu');
})

server.listen('3001' , (err)=>{
        if(err) console.log(`unable to start server`);
        else 
        console.log('Server started at localhost:3001');
    })
