Dola Seedream 5.0 pro (hereinafter referred to as seedream\-5\-0\-pro) is designed for high\-precision image generation scenarios and provides more accurate control over positions and elements. It supports text\-to\-image, single\-image\-to\-image, multi\-reference image generation (up to 10 images), as well as interactive editing for precise coordinate\-based editing and freeform marked\-area editing. This document focuses on the exclusive capabilities of seedream\-5\-0\-pro and helps you quickly get started with the [Image generation API](https://docs.byteplus.com/en/docs/ModelArk/1541523).

<span id="pro-featured"></span>
# Featured capabilities

seedream\-5\-0\-pro adds the following featured capabilities:


<span aceTableMode="list" aceTableWidth="5,4"></span>
|Interactive editing |Native multilingual generation |
|---|---|
|<video src="https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/Seedream_5.0_editing_demo.mp4" controls></video><br><br><br>> Supports specifying edit positions by using coordinates, bounding boxes, arrows, and other markers to precisely edit images, enabling fine\-grained operations such as local element replacement, object positioning, and regional generation. |<span>![图片](https://ark-project.tos-cn-beijing.volces.com/doc_image/seedream_50_pro-part2-tab3-group1-input1.png) </span><br><br>> Adds native text generation support for 14 languages, including Russian, Arabic, Filipino, Thai, Turkish, Korean, Malay, Spanish, Portuguese, Indonesian, French, German, Vietnamese, and Japanese. |


<span id="pro-overview"></span>
# Capability overview

The following table compares the capabilities and parameters of each Seedream model version to help you choose the right model based on your business needs.


<span aceTableMode="list" aceTableWidth="1.5,2,3,3,3,3"></span>
|Model name ||[seedream-5-0-pro](https://console.byteplus.com/ark/region:ark+ap-southeast-1/model/detail?Id=dola-seedream-5-0-pro) |[seedream-5-0-lite](https://console.byteplus.com/ark/region:ark+ap-southeast-1/model/detail?Id=seedream-5-0) |[seedream-4-5](https://console.byteplus.com/ark/region:ark+ap-southeast-1/model/detail?Id=seedream-4-5) |[seedream-4-0](https://console.byteplus.com/ark/region:ark+ap-southeast-1/model/detail?Id=seedream-4-0) |
|---|---|---|---|---|---|
|Model ID ||dola\-seedream\-5\-0\-pro\-260628 |seedream\-5\-0\-260128 (also supports: seedream\-5\-0\-lite\-260128) |seedream\-4\-5\-251128 |seedream\-4\-0\-250828 |
|[Text-to-image](https://docs.byteplus.com/en/docs/ModelArk/1824121#9695d195) ||✓ |✓ |✓ |✓ |
|[Text-to-image batch output](https://docs.byteplus.com/en/docs/ModelArk/1824121#ec79cfda) ||Not supported yet |✓ |✓ |✓ |
|[Single-image or multi-image image-to-image](https://docs.byteplus.com/en/docs/ModelArk/1824121#8bc49063) ||✓ |✓ |✓ |✓ |
|[Single-image or multi-image input with batch output](https://docs.byteplus.com/en/docs/ModelArk/1824121#fc9f85e4) ||Not supported yet |✓ |✓ |✓ |
|[Interactive editing](https://docs.byteplus.com/en/docs/ModelArk/2582774#interactive-edit) ||✓ |✗ |✗ |✗ |
|[Streaming output](https://docs.byteplus.com/en/docs/ModelArk/1824121#e5bef0d7) ||Not supported yet |✓ |✓ |✓ |
|Model parameters |Resolution |1K, 2K |2K, 3K, 4K |2K, 4K |1K, 2K, 4K |
||Output format |png, jpeg |png, jpeg |jpeg |jpeg |
||Prompt optimization mode |Standard mode, fast mode |Standard mode |Standard mode |Standard mode, fast mode |
||Number of generated images |Supports single\-image generation and multiple\-layer image generation |Number of reference images + number of final generated images <= 15 | | |
|IPM rate limit (images/minute) ||500 |500 |500 |500 |


<span id="pro-basic-usage"></span>
# Basic usage

The basic usage of seedream\-5\-0\-pro, including text\-to\-image, image\-to\-image, and multi\-image blending, is the same as other Seedream models. You only need to replace the `model` parameter with `dola-seedream-5-0-pro-260628`. For detailed code examples and instructions, see:


* [Text-to-image](https://docs.byteplus.com/en/docs/ModelArk/1824121#9695d195)

* [Image-to-image](https://docs.byteplus.com/en/docs/ModelArk/1824121#8bc49063)

* [Multi-image blending](https://docs.byteplus.com/en/docs/ModelArk/1824121#4a35e28f)


<span id="interactive-edit"></span>
# Interactive editing

seedream\-5\-0\-pro supports specifying edit positions by using **bounding boxes, points, arrows, annotation boxes, coordinates** , and other methods to precisely generate or modify local areas. For detailed instructions, see [Seedream 5.0 pro interactive editing guide](https://docs.byteplus.com/en/docs/ModelArk/2582775).

<span id="interactive-edit-freeform"></span>
## Example: Freeform marking

Use freehand sketches, doodles, circles, or other markings on the reference image to specify the edit region. The model recognizes the marked area and generates or replaces content within it, while blending the result naturally into the original scene.


<span aceTableMode="list" aceTableWidth="1,1,1"></span>
|Prompt |Input image |Output |
|---|---|---|
|Edit the image based on the hand\-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene. |<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png) </span> |<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_output2.png) </span> |



<Tabs>
<Tab zoneid="grPskzAqim" title="Curl">
<TabTitle>Curl</TabTitle>

```Bash
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "dola-seedream-5-0-pro-260628",
    "prompt": "Edit the image based on the hand-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene.",
    "image": "https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png",
    "size": "2K",
    "output_format": "png",
    "watermark": false
}'
```



* You may replace the Model ID as needed. Refer to [Model list](https://docs.byteplus.com/en/docs/ModelArk/1330310) to find available models.


</Tab>
<Tab zoneid="BWWrSkromm" title="Python">
<TabTitle>Python</TabTitle>

```Python
import os
# Install SDK:  pip install 'byteplus-python-sdk-v2[ark]'
from byteplussdkarkruntime import Ark

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3",
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'),
)

imagesResponse = client.images.generate(
    # Replace with Model ID
    model="dola-seedream-5-0-pro-260628",
    prompt="Edit the image based on the hand-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene.",
    image="https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png",
    size="2K",
    output_format="png",
    response_format="url",
    watermark=False
)

print(imagesResponse.data[0].url)
```



</Tab>
<Tab zoneid="OEEyxhpqdq" title="Java">
<TabTitle>Java</TabTitle>

```Java
package com.ark.sample;


import com.byteplus.ark.runtime.model.images.generation.*;
import com.byteplus.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays;
import java.util.List;
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample {
    public static void main(String[] args) {
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.ap-southeast.bytepluses.com/api/v3") // The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();

        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("dola-seedream-5-0-pro-260628")
                .prompt("Edit the image based on the hand-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene.")
                .image("https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png")
                .size("2K")
                .outputFormat("png")
                .responseFormat(ResponseFormat.Url)
                .watermark(false)
                .build();

        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
```



</Tab>
<Tab zoneid="KVYVjeEnYq" title="Go">
<TabTitle>Go</TabTitle>

```Go
package main

import (
    "context"
    "fmt"
    "os"

    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime/model"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/byteplus"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.ap-southeast.bytepluses.com/api/v3"),
    )
    ctx := context.Background()
    outputFormat := model.OutputFormatPNG

    generateReq := model.GenerateImagesRequest{
       Model:          "dola-seedream-5-0-pro-260628",
       Prompt:         "Edit the image based on the hand-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene.",
       Image:          byteplus.String("https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png"),
       Size:           byteplus.String("2K"),
       OutputFormat:   &outputFormat,
       ResponseFormat: byteplus.String("url"),
       Watermark:      byteplus.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\\n", err)
       return
    }

    fmt.Printf("%s\\n", *imagesResponse.Data[0].Url)
}
```



</Tab>
<Tab zoneid="F3vGrBOF88" title="OpenAI">
<TabTitle>OpenAI</TabTitle>

```Python
import os
from openai import OpenAI

client = OpenAI(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3",
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'),
)

imagesResponse = client.images.generate(
    model="dola-seedream-5-0-pro-260628",
    prompt="Edit the image based on the hand-drawn sketch. Add a stack of realistic magazines or art books in the marked area at the lower left, and add a ceramic cup of coffee with a saucer in the marked area on the right. Remove all sketch lines. Keep the composition unchanged. Let the newly added objects blend naturally into the original scene.",
    size="2K",
    output_format="png",
    response_format="url",
    extra_body = {
        "image": "https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_50_pro_input2.png",
        "watermark": False
    }
)

print(imagesResponse.data[0].url)
```



</Tab>
</Tabs>


<span id="example-coordinate-positioning"></span>
## Example: Coordinate positioning

Add `<point>` or `<bbox>` coordinate tags to the prompt to precisely specify a cross\-image edit area and position the subject. For complete steps and parameter descriptions, see [Seedream 5.0 pro interactive editing guide](https://docs.byteplus.com/en/docs/ModelArk/2582775).


<span aceTableMode="list" aceTableWidth="1,1.5,1"></span>
|Prompt |Input image |Output |
|---|---|---|
|`Use the subject in Image 2 <bbox>118 331 933 871</bbox> to replace the subject in Image 1 <bbox>179 283 796 986</bbox>.` |<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_5.0_input_combined.png) </span> |<span>![图片](https://arkdocs-en.tos-ap-southeast-1.volces.com/images/image-generation/seedream_5.0_output.jpeg) </span> |


<span id="usage-instructions"></span>
## Usage instructions

Interactive editing requires the following inputs: **the image to edit** and a **prompt** that contains location information and editing instructions. Depending on the location method, interactive editing supports the following two forms:


<span aceTableMode="list" aceTableWidth="1,1"></span>
|Form 1: Free\-form marker + natural\-language location |Form 2: Precise coordinate location |
|---|---|
|Mark the edit area on the image to edit by using hand\-drawn sketches, doodles, circles, or other methods, and then describe the marker position and editing intent in natural language in the prompt.<br><br>```JSON```<br>```{```<br>```    "prompt": "Add a TV inside the blue box" // Describe the marked area and editing intent in natural language```<br>```}```<br> |Use a tool to frame the coordinates of the content to edit. For information about how to obtain coordinates, see [Seedream 5.0 pro interactive editing guide](https://docs.byteplus.com/en/docs/ModelArk/2582775). In the prompt, use `<point>` or `<bbox>` coordinate tags to precisely specify the position.<br><br>```JSON```<br>```{```<br>```    "prompt": "Place the subject from Image 1 <bbox>179 283 796 986</bbox> at the position of Image 2 <bbox>118 331 933 871</bbox>" // Use coordinate tags to specify the exact location```<br>```}```<br> |


After preparing the inputs, pass **the image to edit** and the **prompt** to the API to generate the image editing result.

<span id="pro-prompt-optimize"></span>
# Prompt optimization mode

seedream\-5\-0\-pro supports selecting the prompt optimization mode by using the `optimize_prompt_options.mode` parameter:


* `standard` (default): Standard mode. This mode produces higher\-quality content but takes longer.

* `fast`: Fast mode. This mode takes less time to generate content, but the result is slightly lower than standard mode.


<div data-tips="true" data-tips-type="tip" data-tips-is-title="true">Recommendation</div>


<div data-tips="true" data-tips-type="tip">If your business is sensitive to generation latency, we recommend that you use <code>fast</code> mode to reduce waiting time.</div>


```JSON
{
    "optimize_prompt_options": {
        "mode": "fast"
    }
}
```


<span id="pro-output-spec"></span>
# Customize image output specifications

You can configure the following parameters to control image output specifications:


* **size** : Specifies the size of the output image.

* **response_format** : Specifies the return format of the generated image.

* **output_format** : Specifies the file format of the generated image.

* **watermark** : Specifies whether to add a watermark to the output image.


<span id="image-output-size"></span>
### Image output size

The following size specification methods are supported. Do not use the two methods at the same time.

**Method 1: Specify a resolution tier (recommended)** 

Use natural language in the prompt to describe the image aspect ratio, image shape, or image purpose. The model then determines the final image size.


* Default value: `2K`

* Valid values: `1K`, `2K`


When you use Method 1 and describe a specific aspect ratio in the prompt, the actual width and height values mapped by the model are shown in the following table. The model supports more aspect ratios than the standard values listed here. The following table uses common aspect ratios only as examples.


<span aceTableMode="list" aceTableWidth="1,1,1.5"></span>
|Resolution |Aspect ratio |Width and height |
|---|---|---|
|1K |1:1 |1024x1024 |
||4:3 |1152x864 |
||3:4 |864x1152 |
||16:9 |1424x800 |
||9:16 |800x1424 |
||3:2 |1248x832 |
||2:3 |832x1248 |
||21:9 |1568x672 |
|2K |1:1 |2048x2048 |
||4:3 |2368x1776 |
||3:4 |1776x2368 |
||16:9 |2816x1584 |
||9:16 |1584x2816 |
||3:2 |2496x1664 |
||2:3 |1664x2496 |
||21:9 |3136x1344 |


**Method 2: Specify width and height in pixels (** **`widthxheight`** **)** 


* Total pixel range: [`1280x720` (921600), `2048x2048x1.1025` (4624220)]

* Aspect ratio range: [1/16, 16]


<div data-tips="true" data-tips-type="tip" data-tips-is-title="true">Note</div>


<div data-tips="true" data-tips-type="tip">When you use Method 2, both the total pixel range and the aspect ratio range must be met. Total pixels refer to the product of the width and height of a single image, not a separate limit on width or height.</div>



* <div data-tips="true" data-tips-type="tip"><strong>Valid example</strong> : <code>2048x1024</code></div>


   <div data-tips="true" data-tips-type="tip">The total pixel value is 2048x1024=2097152, which falls within [921600, 4624220]. The aspect ratio is 2048/1024=2, which falls within [1/16, 16]. Therefore, this value is valid.   </div>
   

* <div data-tips="true" data-tips-type="tip"><strong>Invalid example</strong> : <code>512x512</code></div>


   <div data-tips="true" data-tips-type="tip">The total pixel value is 512x512=262144, which is below the minimum value of 921600. Therefore, this value is invalid.   </div>
   



<span aceTableMode="list" aceTableWidth="1,1"></span>
|Method 1 |Method 2 |
|---|---|
|```JSON```<br>```{```<br>```    "prompt": "Generate a set of four cohesive illustrations with a 3:2 aspect ratio, centered on the seasonal transformation of the same corner of a courtyard, using a unified style to present the distinct colors, elements, and atmosphere of each season.", // In the prompt, use natural language to describe the aspect ratio, shape, or purpose of the image```<br>```    "size": "2K" // Specifies the resolution of the generated image by using the size parameter```<br>```}```<br> |```JSON```<br>```{```<br>```    "prompt": "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",```<br>```    "size": "2048x2048" // Specifies the width and height of the generated image in pixels```<br>```}```<br> |


<span id="image-output-method"></span>
### Image output method

Set the **response_format** parameter to specify the return method of the generated image:


* `url`: Returns an image download URL.

* `b64_json`: Returns image data as a Base64\-encoded string in JSON format.


```JSON
{
    "response_format": "url"
}
```


<span id="image-file-format"></span>
### Image file format

Set the **output_format** parameter to specify the file format of the generated image:


* `png`

* `jpeg`


```JSON
{
    "output_format": "png"
}
```


<span id="add-a-watermark-to-images"></span>
### Add a watermark to images

Use the **watermark** parameter to control whether to add a watermark to the generated image.


* `false`: Do not add a watermark.

* `true`: Add an "AI\-generated" watermark in the lower\-right corner of the image.


```JSON
{
    "watermark": true
}
```


<span id="pro-limits"></span>
# Limits

**SDK version upgrade**

To ensure that the model works properly, upgrade to the latest SDK version. For more information, see [Install and upgrade SDK](https://docs.byteplus.com/en/docs/ModelArk/1541595).

**Image input limits**


* Image formats: jpeg, png, webp, bmp, tiff, gif, heic, heif

* Image input methods:

   * Image URL: Make sure that the image URL is accessible.

      Example: `https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png`

   * Base64 encoding: Use the following format: `data:image/<image_format>;base64,<base64_image>`. Note that `<image_format>` must be in lowercase, for example, `data:image/png;base64,<base64_image>`.

      To obtain the Base64 encoding of an image, you can use a third\-party tool such as https://base64.guru/converter/encode/image.

* Aspect ratio (width/height): [1/16, 16]

* Width and height (px): \> 14

* Size: No more than 30 MB

* Total pixels: No more than `6000x6000=36000000` px. This limit applies to the product of the width and height of a single image, not to width or height separately.

* Supports up to 10 reference images.


**Retention period**

Image URLs are retained for only 24 hours. They are automatically cleared after they expire. Save generated images in time.

**Rate limit**


* RPM rate limit: The maximum number of images that can be generated per minute for the same model, differentiated by model version, under an account. If this limit is exceeded, image generation returns an error.

* Limits vary by model. For more information, see [Image generation capabilities](https://docs.byteplus.com/en/docs/ModelArk/1330310#d3e5e0eb).




