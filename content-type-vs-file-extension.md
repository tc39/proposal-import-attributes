# Mismatch between Content-Type and file extension on the web

For Node.js or tooling we can make the association between the type of a file and its extension.
For example, it might sound obvious that a file ending with `.js` would correspond to a `application/javascript` file.

Interestingly, Node.js or webpack does allow to arbitrarily redirect module resolution.
Our previous assumption is already not guaranteed. See [#4](https://github.com/tc39/proposal-import-conditions/issues/4).

Moreover, on the web the server needs to send a `Content-Type` header to the client, so that
the client can know how to interpret the ressource being transfered.

The web server could be misconfigured (or malicious) and send a wrong `Content-Type` header to the client. For example,
a file with the extension `.css` could end up being a JavaScript file and be evaluated by the client.

This proposal will allow the developer to assert that their ressources will be interpreted correctly.

## Analysis

This analysis demonstrates that on internet (as seen by Cloudflare at least) the
file extension may not be served with its corresponding mimetype.

Considering Cloudflare's scale, even a very small percentage can reprensent a
lot of requests.

### `.js`

ContentType header for files ending with `.js`:

| mimetype                 | %                |
|--------------------------|------------------|
| application/javascript   | 61.82757604      |
| empty                    | 20.11803875      |
| text/javascript          | 6.980723063      |
| application/x-javascript | 6.544467745      |
| text/html                | 2.518384529      |
| application/json         | 1.10533001       |
| text/plain               | 0.697906732      |
| unknown                  | 0.0559300176     |
| application/xml          | 0.03160118805    |
| application/octet-stream | 0.02865532793    |
| application/ecmascript   | 0.005831504263   |
| video/mp2t               | 0.004634268927   |
| image/png                | 0.002653832304   |
| image/gif                | 0.001813563585   |
| text/x-c                 | 0.0009681784766  |
| text/css                 | 0.0007277081314  |
| image/jpeg               | 0.0006049147636  |
| image/webp               | 0.000348701679   |
| video/mp4                | 0.00006926805361 |
| text/x-asm               | 0.00003384688983 |


### `.json`

ContentType header for files ending with `.json`:

| mimetype                 | %                  |
|--------------------------|--------------------|
| application/json         | 67.62137903        |
| text/html                | 14.67113587        |
| empty                    | 8.77703774         |
| text/plain               | 3.087589957        |
| unknown                  | 3.045687106        |
| application/octet-stream | 2.034226667        |
| application/javascript   | 0.3646704983       |
| text/javascript          | 0.2912919013       |
| application/xml          | 0.09035178168      |
| application/x-javascript | 0.01219521326      |
| image/gif                | 0.001177305887     |
| image/jpeg               | 0.001159044755     |
| text/css                 | 0.0003426647608    |
| image/png                | 0.00004726410493   |
| application/xhtml+xml    | 0.00003974481551   |
| text/xml                 | 0.00003974481551   |
| image/jpg                | 0.00002255786826   |
| application/pdf          | 0.000005370921015  |
| application/zip          | 0.000003222552609  |
| audio/x-wav              | 0.0000001074184203 |

