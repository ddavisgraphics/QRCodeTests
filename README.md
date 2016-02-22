# QR Code Tests

This is done in a setup for vagrant with EngineAPI PHP, but doesn't need to be this could have been a static test.  The files needed are described below for your personal need.  

## HTML 
```
<!DOCTYPE HTML>
<html>
    <head>
        <meta name="viewport" content="initial-scale=1.0">
        <meta http-equiv="Content-type" content="text/html; charset=utf-8">
        <title> Camera Application Test</title>
    </head>

    <body>
        <select class="imageType" id="imageType">
            <option value=""> Select an Image Type </option>
            <option value="pic"> Picture </option>
            <option value="qr"> QR Code </option>
        </select>

        <!-- Inputs -->
        <input type="file" accept="image/*" id="pictureField" src="">

        <div style="width:300px;">
            <img id="insertedImage" style="width:100%; height:auto;" />
        </div>


    <!-- JS Dependencies -->
    <script src="/includes/templateIncludes/js/bower_components/jquery/dist/jquery.min.js"></script>
    <script src="/includes/templateIncludes/js/bower_components/qcode-decoder/build/qcode-decoder.min.js"></script>
    <script src="/includes/templateIncludes/js/main.js"></script>

    </body>
</html>
``` 

JS 
``` 
// set vars and instantiate new classes
    var QRCode   = false;

// Create Event Handles
    $("#imageType").on("change", pictureType);
    ("#pictureField").on("change", handleImageUpload);

// Create Functions for Handlers
//
function handleImageUpload(event){
    var qrReader = new QCodeDecoder();
    var reader   = new FileReader();

    // get files
    var uploadedFiles = event.target.files;
    var imageFile     = event.target.files[0];
    var dataURI = "";

    // create a preview
    var imagePreview = $('#insertedImage');

    // file exsists load the image into the reader
    if(imageFile){

        // create reader events for file
        reader.onload = function(event) {
            dataURI = reader.result;
            imagePreview.attr('src', dataURI);

            // read the qr code if the image is a QR Image
            if(QRCode){
                console.log('QRCode happening');
                console.log(dataURI);
                qrReader.decodeFromImage(dataURI,function(error,result){
                    window.location.href = result;
                });
            }
        };

        reader.onerror = function(event) {
            console.error("File could not be read! Code " + event.target.error.code);
        };

        reader.readAsDataURL(imageFile);
    }
}

function pictureType(event){
    value = $(this).val();
    if(value == 'qr'){
        QRCode = true;
        console.log(QRCode + ": QRcode");
    } else {
        QRCode = false;
        console.log(QRCode + ": QRcode");
    }
}
``` 

## Bower Components 
- QR Code Decoder : http://cirocosta.github.io/qcode-decoder/
- JQuery : https://jquery.com/download/ 
