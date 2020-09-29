# Axios for Github Action

You can use this action to perform REST API base on [axios](https://github.com/axios/axios) module. 


# Usage
```yaml
name: Example of axios action

on: [ push ]
jobs:
    test-axios-action:
        name: 'Perform REST API'
        runs-on: ubuntu-latest
        steps:
            - name: 'Call API'
              uses: actionsflow/axios@v1
              with:
                # The target URL
                # Required: true if custom-config is not set
                url: https://reqres.in/api/users
                
                # The request method, basically it's one of GET|POST|PUT|PATCH
                # Default is GET 
                method: 'POST'

                # List of response status codes to be accepted, else it will set the job to be failed
                # If more than one value is needed, you can use comma(,) as seperator
                # In this case if the response status code is not one of 200, 201 and 204, the job will be failed
                # Default is 200,201,204
                accept: 200,201,204

                # Headers can be passed through json object string 
                # support escape string, use <<<${{env.TEST}}>>>
                headers: '{ "custom-header": "value" }'

                # Content-Type Header value, if body is json string, then content-type default value is application/json
                content-type: application/json

                # Params can be passed through json object string 
                # support escape string, use <<<${{env.TEST}}>>>
                params: '{ "param1": "value", "param2": "value2" }'
                
                # Body request
                # Apply only to POST|PUT request
                # support escape string, use <<<${{env.TEST}}>>>
                data: '{ "name": "breeze",  "job": "<<<${{env.TEST}}>>>" }'

                # Request timeout (millisec)
                # default is `0` (no timeout)
                timeout: 10000
                
                # Basic authentication using username and password
                # This will override the Authorization header, for example Authorization: Basic QWxhZGRpbjpPcGVuU2VzYW1l
                # Format => username:password
                basic-auth: ${{ secrets.curl_auth_username }}:${{ secrets.curl_auth_password }}
                
                # The authentication using token
                # This will override the Authorization header, for example Authorization: Bearer QWxhZGRpbjpPcGVuU2VzYW1l
                bearer-token: ${{ secrets.bearer_token }}

                # If you want to use proxy with the request, you can use proxy-url
                # Format => host:port
                proxy-url: https://proxy-url:3000

                # If the proxy host requires the authentication, you can use proxy-auth to pass credentials 
                # Format => username:password
                proxy-auth: ${{ secrets.proxy_auth_username }}:${{ secrets.proxy_auth_password }}

                # If it is set to true, it will show the response log in the Github UI
                # Default: false
                is_debug: false

                # If you want to use axios config directly, you can pass config file to the action
                # The file is just basically a json file that has the same format as axios config https://github.com/axios/axios#request-config
                # If this input is set, it will ignore other inputs that related to the config
                # The path file is start from root directory of the repo
                custom-config: .github/workflows/curl-config.json
  
  ```


# Outputs 

```javascript
{
  // `data` is the response that was provided by the server
  "data": "{}",

  // `status` is the HTTP status code from the server response
  "status": 200,

  // `headers` the HTTP headers that the server responded with
  // All header names are lower cased and can be accessed using the bracket notation.
  "headers": '{"content-type":"application/json; charset=utf-8"}',

}

```


# Use Response 
```yaml
name: Example of axios action
on: [ push ]
jobs:
    test-curl-action:
        name: 'Perform REST API'
        runs-on: ubuntu-latest
        steps:
            - name: 'Call API'
              uses: actionsflow/axios@v1
              id: api
              with:
                url: https://reqres.in/api/users
                method: 'POST'
                body: '{ "name": "breeze",  "job": "devops" }'
            - run: echo ${{ steps.api.outputs.status }}
            - run: echo ${{ steps.api.outputs.data }}
            - run: echo ${{ steps.api.outputs.headers }}
  ```


