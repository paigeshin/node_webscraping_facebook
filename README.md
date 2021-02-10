# Dependency

```bash
npm i request request-promsie
```

# Glitch 발견

- firefox로 들어가면 facebook에 javascript가 없어진 것을 확인
- request를 할 때 user-agent를 firefox인것 처럼 가장하여 페이스북에 로그인 request를 fake한다
- 필요한 데이터를 가져온다

# Project

```jsx
const defaultOptions = {
    headers: {
        'User-Agent': 
        'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.14; rv:63.0) Gecko/20100101 Firefox/63.0'
    },
    jar: true //save cookies
}

const request = require('request-promise').defaults(defaultOptions)
const fs = require('fs')

async function main() {

    const options = {
        method: 'POST',
        uri: 'https://m.facebook.com/login/blabla',
        form: {
            email: 'example@gmail.com',
            pass: 'password'
        },
        simple: false 
    }

    try {
        const result = await request(options)
        console.log(result.headers.location)
        const homepage = await request.get(result.headers.location)
        writeFile(homepage)
    } catch(error) {
        console.error(error)
    }

}

function writeFile(body) {
    fs.writeFile('./test.html', body, (err) => {
        if(err) {
            console.error(err)
        }
        console.log('html was saved')
    })
}

main()
```