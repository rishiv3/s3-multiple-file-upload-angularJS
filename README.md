# AWS S3 Multiple File Upload with AngularJS
Upload multiple files on AWS S3 and storing the url to Firebase using Angular


You can do this by AWS S3 Cognito
try this link here :

http://docs.aws.amazon.com/AWSJavaScriptSDK/guide/browser-examples.html#Amazon_S3

**Also try this code**

Just change Region, IdentityPoolId and Your bucket name 

````HTML
<!DOCTYPE html>
<html>

    <head>
        <title>AWS S3 File Upload</title>
        <script src="https://sdk.amazonaws.com/js/aws-sdk-2.1.12.min.js"></script>
    </head>

    <body>
        <input type="file" id="file-chooser" />
        <button id="upload-button">Upload to S3</button>
        <div id="results"></div>
        <script type="text/javascript">
        AWS.config.region = 'your-region'; // 1. Enter your region

        AWS.config.credentials = new AWS.CognitoIdentityCredentials({
            IdentityPoolId: 'your-IdentityPoolId' // 2. Enter your identity pool
        });

        AWS.config.credentials.get(function(err) {
            if (err) alert(err);
            console.log(AWS.config.credentials);
        });

        var bucketName = 'your-bucket'; // Enter your bucket name
        var bucket = new AWS.S3({
            params: {
                Bucket: bucketName
            }
        });

        var fileChooser = document.getElementById('file-chooser');
        var button = document.getElementById('upload-button');
        var results = document.getElementById('results');
        button.addEventListener('click', function() {

            var file = fileChooser.files[0];

            if (file) {

                results.innerHTML = '';
                var objKey = 'testing/' + file.name;
                var params = {
                    Key: objKey,
                    ContentType: file.type,
                    Body: file,
                    ACL: 'public-read'
                };

                bucket.putObject(params, function(err, data) {
                    if (err) {
                        results.innerHTML = 'ERROR: ' + err;
                    } else {
                        listObjs();
                    }
                });
            } else {
                results.innerHTML = 'Nothing to upload.';
            }
        }, false);
        function listObjs() {
            var prefix = 'testing';
            bucket.listObjects({
                Prefix: prefix
            }, function(err, data) {
                if (err) {
                    results.innerHTML = 'ERROR: ' + err;
                } else {
                    var objKeys = "";
                    data.Contents.forEach(function(obj) {
                        objKeys += obj.Key + "<br>";
                    });
                    results.innerHTML = objKeys;
                }
            });
        }
        </script>
    </body>

</html>
````


# Firebase Docs 

for more information - https://firebase.google.com/docs/

## Setup firebsae
https://firebase.google.com/docs/web/setup
