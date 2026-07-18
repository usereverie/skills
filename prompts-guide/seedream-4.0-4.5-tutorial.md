Seedream 4.5 and 4.0 natively support text, single-image, and multi-image inputs, enabling diverse workflows such as multi-image fusion based on subject consistency, image editing, and batch image generation. This provides creators with greater flexibility and control over the image-creation process.
This document uses Seedream 4.5 as an example to illustrate how to use <a href="https://docs.byteplus.com/en/docs/ModelArk/1541523">Image generation API</a> for image creation. To use the Seedream 4.0 model, replace the model field in the code samples below with `seedream-4-0-250828`.
<span id="2cf5cace"></span>
# Capabilities

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);">

Domain


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Input


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Output


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3348576323987539);">

Multi-reference image-to-image generation
> Input multiple images as reference, blend styles and elements to generate new images






</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3317423676012461);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2198d4bef000400bbfea18025850ed82~tplv-goo7wpa0wc-image.image" width="930px" /></div>

> Replace the clothing in image 1 with the outfit from image 2.


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/461d4bf2a014454fbeda72f27d706ffe~tplv-goo7wpa0wc-image.image" width="1280px" /></div>




</div>
</div>


---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3311535974600903);">

Sequential batch image generation
> Based on text and images entered by the user, generate a set of content-related images




</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3320553846383162);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a215e8241dd94f50901948790da121e1~tplv-goo7wpa0wc-image.image" width="930px" /></div>

> Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3366910179015934);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/98c9e2c30dbb425aa25380c821e289ca~tplv-goo7wpa0wc-image.image" width="1200px" /></div>




</div>
</div>

<span id="9278b81b"></span>
# Model Selection

* Seedream 4.5, ByteDance's latest and most advanced image generation model, delivers stronger performance across key areas. It offers improved consistency in editing—such as preserving subject details, lighting, and color tones—along with enhanced portrait refinement and small-text generation. The model also features significantly strengthened multi-image composition capabilities. With ongoing optimizations in reasoning and visual aesthetics, it can bring creative ideas to life more accurately and artistically.
* Seedream 4.0 image generation model balances cost and output quality, suitable for scenarios requiring general image generation.


| | | | | | | | \
|**Model Name** |\
|<div style="width:150px"></div> |**Version** |\
| |<div style="width:150px"></div> |**Model ID** |\
| | |<div style="width:150px"></div> |**Model capabilities** |\
| | | |<div style="width:200px"></div> |**Max Images per Minute** |\
| | | | |<div style="width:150px"></div> |**Price** (USD / image) |\
| | | | | |<div style="width:150px"></div> |**Free Credit** (Piece) |
|---|---|---|---|---|---|---|
| | | | | | | | \
|seedream-4.5 |251128`Highly recommended` |seedream-4-5-251128 |Text-to-Image |\
| | | |Image-to-Image |\
| | | | |\
| | | |* Single Image-to-Image |\
| | | |* Generate images with multiple reference images |\
| | | | |\
| | | |Generate a batch of images |\
| | | | |\
| | | |* Generate a batch of images from text |\
| | | |* Generate a batch of images from a single image |\
| | | |* Generate a batch of images from multiple reference images |500 |[Image Generation ](/docs/ModelArk/1544106#c02be6ee) |200 |
| | | |^^| | | | \
|[seedream-4.0](/docs/ModelArk/1824718) |250828 |\
| |`recommend` |seedream-4-0-250828 | |500 |[Image Generation ](/docs/ModelArk/1544106#c02be6ee) |200 |\
| | | | | | | |



<span id="88612aa1"></span>
# Prerequisites

* Obtain the API key used to authenticate online inference requests.
   * See [1. Obtaining and Configuring API Key](/docs/ModelArk/1399008#b00dee71) for more information.

* [Obtain an API key](https://console.byteplus.com/ark/apiKey)
* [Enable the model service](https://console.byteplus.com/ark/openManagement)
* Obtain the required Model ID from  [Model List](/docs/ModelArk/1330310)


<span id="386b6ea2"></span>
# Quick start
You can try the image generation feature on the ModelArk platform using [API Explorer](https://api.byteplus.com/api-explorer/?action=ImageGenerations&groupName=Image%20Generation%20API&serviceCode=ark&version=2024-01-01), It supports custom parameters configuration (e.g. watermark settings, output image size), and effect & performance evaluation.
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2eea8e183d5d424cbd1f707bef9351d1~tplv-goo7wpa0wc-image.image =2362x)
<span id="e36d7d78"></span>
# Basic usage
<span id="9695d195"></span>
## Text-to-Image (Text Input, Single Image Output)
Provide clear and accurate text instructions to the model to quickly generate a high-quality image that matches the description.

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6671732522796352);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.33282674772036475);margin-left: 16px;">

Output


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6656534954407295);">

Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.



</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.3343465045592705);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/00fb66006eb84b16965b620b6e1f2d78~tplv-goo7wpa0wc-image.image" width="1280px" /></div>



</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="hgh3pdSlav"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.",
    "size": "2K",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="Ngr6CASoIX"><RenderMd content={`\`\`\`Python
import os
# Install SDK:pip install byteplus-python-sdk-v2  .
from byteplussdkarkruntime import Ark 

client = Ark(
    #The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
)
 
imagesResponse = client.images.generate( 
    #Replace with Model ID
    model="seedream-4-5-251128",
    prompt="Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.",
    size="2K",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="Vni9MBBBu0"><RenderMd content={`\`\`\`Java
package com.ark.sample;

import com.byteplus.ark.runtime.model.images.generation.GenerateImagesRequest;
import com.byteplus.ark.runtime.model.images.generation.ImagesResponse;
import com.byteplus.ark.runtime.model.images.generation.ResponseFormat;
import com.byteplus.ark.runtime.model.images.generation.Size;
import com.byteplus.ark.runtime.service.ArkService;
import okhttp3.ConnectionPool;
import okhttp3.Dispatcher;

import java.util.Arrays; 
import java.util.List; 
import java.util.concurrent.TimeUnit;

public class ImageGenerationsExample { 
    public static void main(String[] args) {
        // Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
        String apiKey = System.getenv("ARK_API_KEY");
        ConnectionPool connectionPool = new ConnectionPool(5, 1, TimeUnit.SECONDS);
        Dispatcher dispatcher = new Dispatcher();
        ArkService service = ArkService.builder()
                .baseUrl("https://ark.ap-southeast.bytepluses.com/api/v3") //The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
                
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                .model("seedream-4-5-251128") //Replace with Model ID
                .prompt("Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.")
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="LUwaajAQjC"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime/model"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/byteplus"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        // Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation
        arkruntime.WithBaseUrl("https://ark.ap-southeast.bytepluses.com/api/v3"),
    )    
    ctx := context.Background()

    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128", // Replace with Model ID
       Prompt:         "Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.",
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\\n", err)
       return
    }

    fmt.Printf("%s\\n", *imagesResponse.Data[0].Url)
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="oFZpToAPj8"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    #The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    #Replace with Model ID
    model="seedream-4-5-251128",
    prompt="Vibrant close-up editorial portrait, model with piercing gaze, wearing a sculptural hat, rich color blocking, sharp focus on eyes, shallow depth of field, Vogue magazine cover aesthetic, shot on medium format, dramatic studio lighting.",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": false,
    },
) 
 
print(imagesResponse.data[0].url)

\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="8bc49063"></span>
## Image-to-Image (Single Image Input, Single Image Output)
Edit an existing image using text instructions, including adding or removing elements, changing style or texture, adjusting color tone, and modifying the background, perspective, or size.

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Input image


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Output


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3344961722488038);">

Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3321038277511962);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/816153e67d3c4478886276154d78b22e~tplv-goo7wpa0wc-image.image" width="930px" /></div>




</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0829972712544f95917464b15723b189~tplv-goo7wpa0wc-image.image" width="1280px" /></div>




</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="NX9235TML4"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.",
    "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png",
    "size": "2K",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="yCkLTfjAmX"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
)
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="seedream-4-5-251128", 
    prompt="Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.",
    image="https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png",
    size="2K",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="INwWWqvzFm"><RenderMd content={`\`\`\`Java
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
                .model("seedream-4-5-251128") // Replace with Model ID
                .prompt("Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.")
                .image("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png")
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
                
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="TIWRUguSlL"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
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

    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.",
       Image:          byteplus.String("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png"),
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\\n", err)
       return
    }

    fmt.Printf("%s\\n", *imagesResponse.Data[0].Url)
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="uUY5McaseJ"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

imagesResponse = client.images.generate( 
    model="seedream-4-5-251128",
    prompt="Keep the model's pose and the flowing shape of the liquid dress unchanged. Change the clothing material from silver metal to completely transparent clear water (or glass). Through the liquid water, the model's skin details are visible. Lighting changes from reflection to refraction.",
    size="2K",
    response_format="url",
    extra_body = {
        "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imageToimage.png",
        "watermark": False
    }
) 

print(imagesResponse.data[0].url)
\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```


<span id="4a35e28f"></span>
## Multi-Image Blending (Multi-Image Input, Single Image Output)
Generate a new image by blending styles and visual elements from your prompt and multiple reference images. For example, you can merge clothing, shoes, and accessories with model photos to create outfit images, or combine people with landscapes to produce portrait scenes.

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.19634146341463415);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2658536585365853);margin-left: 16px;">

Input image 1


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2695121951219512);margin-left: 16px;">

Input image 2


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2682926829268293);margin-left: 16px;">

Output


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.1951219512195122);">

Replace the clothing in image 1 with the outfit from image 2.


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2695121951219512);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4b4464161cf3463db6f9463b10939178~tplv-goo7wpa0wc-image.image" width="1024px" /></div>



</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.26707317073170733);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c23d1b0528a14cb08b684307eabdcc9b~tplv-goo7wpa0wc-image.image" width="2046px" /></div>



</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2682926829268293);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/461d4bf2a014454fbeda72f27d706ffe~tplv-goo7wpa0wc-image.image" width="1280px" /></div>



</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="FriZ7DxEdr"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Replace the clothing in image 1 with the outfit from image 2.",
    "image": ["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imagesToimage_2.png"],
    "sequential_image_generation": "disabled",
    "size": "2K",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="ZHj7MzmZfq"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="seedream-4-5-251128",
    prompt="Replace the clothing in image 1 with the outfit from image 2.",
    image=["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imagesToimage_2.png"],
    size="2K",
    sequential_image_generation="disabled",
    response_format="url",
    watermark=False
) 
 
print(imagesResponse.data[0].url)
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="K2p77cGhYy"><RenderMd content={`\`\`\`Java
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
                .model("seedream-4-5-251128") // Replace with Model ID
                .prompt("Replace the clothing in image 1 with the outfit from image 2.")
                .image(Arrays.asList(
                    "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimage_1.png",
                    "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imagesToimage_2.png"
                ))
                .size("2K")
                .sequentialImageGeneration("disabled")
                .responseFormat(ResponseFormat.Url)
                .stream(false)
                .watermark(false)
                .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        System.out.println(imagesResponse.getData().get(0).getUrl());

        service.shutdownExecutor();
    }
}
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="iJPiyBvRfh"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
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

    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Replace the clothing in image 1 with the outfit from image 2.",
       Image:         []string{
           "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimage_1.png",
           "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imagesToimage_2.png",
       },
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
    }

    imagesResponse, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
       fmt.Printf("generate images error: %v\\n", err)
       return
    }

    fmt.Printf("%s\\n", *imagesResponse.Data[0].Url)
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="aRSmfXEhvD"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="seedream-4-5-251128",
    prompt="Replace the clothing in image 1 with the outfit from image 2.",
    size="2K",
    response_format="url",
    
    extra_body = {
        "image": ["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimage_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_5_imagesToimage_2.png"],
        "watermark": False,
        "sequential_image_generation": "disabled",
    }
) 
 
print(imagesResponse.data[0].url)
\`\`\`


`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="b4da5e23"></span>
## Batch image output 
Generate a set of thematically related images—such as comic storyboards or brand visuals—using one or more images combined with text descriptions.
Specify the parameter **sequential_image_generation**  as `auto`.
<span id="b5f76bc7"></span>
### Text-to-Batch-Image（Text Input, Batch Image Output)

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6702127659574468);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.32978723404255317);margin-left: 16px;">

Output (four pictures will be generated)


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.6717325227963526);">

Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.


</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.3282674772036474);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9df00a4f19e84efc9946f7033a4cf80d~tplv-goo7wpa0wc-image.image" width="1728px" /></div>



</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="ghJcLXmb5F"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="sUyNp8BaZh"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 
from byteplussdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation .
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="seedream-4-5-251128", 
    prompt="Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="zdHKZzzgCp"><RenderMd content={`\`\`\`Java
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
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("seedream-4-5-251128")  // Replace with Model ID
                 .prompt("Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }


            service.shutdownExecutor();
        }
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="cgW3ffsshw"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
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
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v\\n", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s\\n", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:\\n", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s\\n", i+1, image.Size, url)
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="U2QnkkBH6X"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation .
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="seedream-4-5-251128",
    prompt="Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": False,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
    },
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="a80c411f"></span>
### Image-to-Batch-Image (Single Image Input, Batch Image Output)

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Input image


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

Output (four pictures will be generated)


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);">

Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.


</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c724450228a94a909580c0400fbf503b~tplv-goo7wpa0wc-image.image" width="1280px" /></div>




</div>
<div style="flex-shrink: 0;width: calc((100% - 32px) * 0.3333);margin-left: 16px;">

<div style="text-align: center"><img src="https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f2d33219328149f58552dfd095b5f502~tplv-goo7wpa0wc-image.image" width="1200px" /></div>



</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="Q9Kgwu0AGv"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.",
    "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages.png",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="oCEunOU05w"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2  .
from byteplussdkarkruntime import Ark 
from byteplussdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation .
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID .
    model="seedream-4-5-251128",
    prompt="Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.",
    image="https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages.png",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="cBG2l2JW1m"><RenderMd content={`\`\`\`Java
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
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("seedream-4-5-251128") // Replace with Model ID
                 .prompt("Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.")
                 .image("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages.png")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }


            service.shutdownExecutor();
        }
    }
}
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="doiwTGGUVy"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
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
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.",
       Image:          byteplus.String("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages.png"),
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v\\n", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s\\n", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:\\n", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s\\n", i+1, image.Size, url)
    }
}
\`\`\`


* 您可按需替换 Model ID。Model ID 查询见 [模型列表](/docs/82379/1330310)。
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="g4yq05WtjJ"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="seedream-4-5-251128", 
    prompt="Using this LOGO as a reference, create a visual design system for an outdoor sports brand named GREEN, including packaging bags, hats, cards, lanyards, etc. Main visual tone is green, with a fun, simple, and modern style.", 
    size="2K",
    response_format="url",
    extra_body={
        "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages.png",
        "watermark": False,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
    }   
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="ef168e47"></span>
### Multi-Image-to-Batch-Image (Multi-Image Input, Batch-Image Output)

---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.20121951219512196);">

Prompt


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2682926829268293);margin-left: 16px;">

Input picture 1


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.26265877035767166);margin-left: 16px;">

Input picture 2


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.26782903452037715);margin-left: 16px;">

Output (three pictures will be generated)


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.19728434504792333);">

Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.26916932907348246);margin-left: 16px;">

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/77024d8e03f24862b066bfc385301120~tplv-goo7wpa0wc-image.image =1446x)


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2675718849840255);margin-left: 16px;">

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2cbc5cf5a68d44899fc52f177fb9cf51~tplv-goo7wpa0wc-image.image =1446x)


</div>
<div style="flex-shrink: 0;width: calc((100% - 48px) * 0.2659744408945687);margin-left: 16px;">

![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/6971832f48384aea82b6009006cd3e56~tplv-goo7wpa0wc-image.image =1200x)


</div>
</div>


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="ylpaPrI1uT"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.",
    "image": ["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_2.png"],
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 3
    },
    "size": "2K",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="OSwqzRBhBb"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2  .
from byteplussdkarkruntime import Ark 
from byteplussdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    # Replace with Model ID
    model="seedream-4-5-251128",
    prompt="Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.",
    image=["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_2.png"],
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=3),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="VBzjTFSnS9"><RenderMd content={`\`\`\`Java
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

        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(3);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("seedream-4-5-251128") // Replace with Model ID
                 .prompt("Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.")
                 .image(Arrays.asList(
                     "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_1.png",
                     "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_2.png"
                 ))
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);

        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }


            service.shutdownExecutor();
        }
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="sx0G7juHzf"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
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
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 3
    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.",
       Image:         []string{
           "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_1.png",
           "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_2.png",
       },

       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v\\n", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s\\n", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:\\n", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s\\n", i+1, image.Size, url)
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="GCGmVY3kzD"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="seedream-4-5-251128", 
    prompt="Generate 3 images of a girl and a cow plushie happily riding a roller coaster in an amusement park, depicting morning, noon, and night.",
    size="2K",
    response_format="url",
    extra_body={
        "image": ["https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_1.png", "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imagesToimages_2.png"],
        "watermark": False,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 3
        },
    }   
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="9971b247"></span>
## **Prompt recommendations**

* Use coherent natural language to describe the **subject + action + environment**. If aesthetics matter, include descriptors of **style,** **color,** **lighting,** or **composition**. For details, see [Seedream 4.0-4.5 Tutorial](/docs/ModelArk/1824121).
* Keep text prompts under 600 English words. Very long prompts may scatter the information, causing the model to overlook details and focus only on key points, which can result in missing elements in the generated image. 

<span id="4d900593"></span>
# Advanced usage
<span id="914db3a9"></span>
## Streaming output
The Seedream 4.5 and Seedream 4.0 models support streaming image generation. Results are returned as soon as an image is created, enabling faster browsing and improving end-user experience.
Enable streaming output mode by setting the **stream**  parameter to `true`.
![Image](https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3a6bf2b6c3f0493e8eef28e76ce62784~tplv-goo7wpa0wc-image.image =2044x)

```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="XTbEpLzdqD"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-5-251128",
    "prompt": "Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops",
    "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages_1.png",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "size": "2K",
    "stream": true,
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="LKBXScHzts"><RenderMd content={`\`\`\`Python
import os
# Install SDK:  pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 
from byteplussdkarkruntime.types.images.images import SequentialImageGenerationOptions

client = Ark(
    # The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

if __name__ == "__main__":
    stream = client.images.generate(
        # Replace with Model ID
        model="seedream-4-5-251128",
        prompt="Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops",
        image="https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages_1.png",
        size="2K",
        sequential_image_generation="auto",
        sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
        response_format="url",
        stream=True,
        watermark=False
    )
    for event in stream:
        if event is None:
            continue
        if event.type == "image_generation.partial_failed":
            print(f"Stream generate images error: {event.error}")
            if event.error is not None and event.error.code.equal("InternalServiceError"):
                break
        elif event.type == "image_generation.partial_succeeded":
            if event.error is None and event.url:
                print(f"recv.Size: {event.size}, recv.Url: {event.url}")
        elif event.type == "image_generation.completed":
            if event.error is None:
                print("Final completed event:")
                print("recv.Usage:", event.usage)
        elif event.type == "image_generation.partial_image":
            print(f"Partial image index={event.partial_image_index}, size={len(event.b64_json)}")
\`\`\`

`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="nSiBA53ixF"><RenderMd content={`\`\`\`Java
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
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("seedream-4-5-251128") //Replace with Model ID .
                 .prompt("Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops")
                 .image("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages_1.png")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .stream(true)
                 .watermark(false)
                 .build();
        System.out.println(generateRequest.toString());
        
        service.streamGenerateImages(generateRequest)
                .doOnError(Throwable::printStackTrace)
                .blockingForEach(
                        choice -> {
                            if (choice == null) return;
                            if ("image_generation.partial_failed".equals(choice.getType())) {
                                if (choice.getError() != null) {
                                    System.err.println("Stream generate images error: " + choice.getError());
                                    if (choice.getError().getCode() != null && choice.getError().getCode().equals("InternalServiceError")) {
                                        throw new RuntimeException("Server error, terminating stream.");
                                    }
                                }
                            }
                            else if ("image_generation.partial_succeeded".equals(choice.getType())) {
                                if (choice.getError() == null && choice.getUrl() != null && !choice.getUrl().isEmpty()) {
                                    System.out.printf("recv.Size: %s, recv.Url: %s%n", choice.getSize(), choice.getUrl());
                                }
                            }
                            else if ("image_generation.completed".equals(choice.getType())) {
                                if (choice.getError() == null && choice.getUsage() != null) {
                                    System.out.println("recv.Usage: " + choice.getUsage().toString());
                                }
                            }
                        }
                );
        service.shutdownExecutor();
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="cBRYF5Irq9"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "io"
    "os"
    "strings"
    
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
    
    var sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages := 4
    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-5-251128",
       Prompt:         "Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops",
       Image:          byteplus.String("https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages_1.png"),
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
    }
    
    stream, err := client.GenerateImagesStreaming(ctx, generateReq)
    if err != nil {
       fmt.Printf("call GenerateImagesStreaming error: %v\\n", err)
       return
    }
    defer stream.Close()
    for {
       recv, err := stream.Recv()
       if err == io.EOF {
          break
       }
       if err != nil {
          fmt.Printf("Stream generate images error: %v\\n", err)
          break
       }
       if recv.Type == "image_generation.partial_failed" {
          fmt.Printf("Stream generate images error: %v\\n", recv.Error)
          if strings.EqualFold(recv.Error.Code, "InternalServiceError") {
             break
          }
       }
       if recv.Type == "image_generation.partial_succeeded" {
          if recv.Error == nil && recv.Url != nil {
             fmt.Printf("recv.Size: %s, recv.Url: %s\\n", recv.Size, *recv.Url)
          }
       }
       if recv.Type == "image_generation.completed" {
          if recv.Error == nil {
             fmt.Printf("recv.Usage: %v\\n", *recv.Usage)
          }
       }
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="FViNpPXUO3"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    # The base URL for model invocation .
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 

if __name__ == "__main__":
    stream = client.images.generate(
        model="seedream-4-5-251128",
        prompt="Referring to Figure 1, generate four images with characters wearing sunglasses, riding motorcycles, wearing hats, and holding lollipops",
        size="2K",
        response_format="b64_json",
        stream=True,
        extra_body={
            "image": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/seedream4_imageToimages_1.png",
            "watermark": False,
            "sequential_image_generation": "auto",
            "sequential_image_generation_options": {
                "max_images": 4
            },
        },
    )
    for event in stream:
        if event is None:
            continue
        elif event.type == "image_generation.partial_succeeded":
            if event.b64_json is not None:
                print(f"size={len(event.b64_json)}, base_64={event.b64_json}")
        elif event.type == "image_generation.completed":
            if event.usage is not None:
                print("Final completed event:")
                print("recv.Usage:", event.usage)
\`\`\`

`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="6b32fe21"></span>
## Prompt optimization control
Set the **optimize_prompt_options.mode** parameter to choose between the `standard` mode and `fast` mode to optimize prompts for different requirements of picture quality and generation speed.

* To balance generation speed and image quality, Seedream 4.0 allows you to set **optimize_prompt_options.mode** to `fast` **** to significantly increase generation speed, though this will come at the cost of some image quality.
*  Seedream 4.5 focuses on high-quality image generation and only supports `standard` mode.


```mixin-react
return (<Tabs>
<Tabs.TabPane title="Curl" key="altX4YHIcE"><RenderMd content={`\`\`\`Plain Text
curl https://ark.ap-southeast.bytepluses.com/api/v3/images/generations \\
  -H "Content-Type: application/json" \\
  -H "Authorization: Bearer $ARK_API_KEY" \\
  -d '{
    "model": "seedream-4-0-250828",
    "prompt": "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    "size": "2K",
    "sequential_image_generation": "auto",
    "sequential_image_generation_options": {
        "max_images": 4
    },
    "optimize_prompt_options": {
        "mode": "fast"
    },
    "stream": false,
    "response_format": "url",
    "watermark": false
}'
\`\`\`


* You may replace the Model ID as needed. Refer to [Model List](/docs/ModelArk/1330310) to find available models.
`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Python" key="MF5O1DIeMW"><RenderMd content={`\`\`\`Python
import os
# Install SDK:pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 
from byteplussdkarkruntime.types.images.images import SequentialImageGenerationOptions
from byteplussdkarkruntime.types.images.images import OptimizePromptOptions

client = Ark(
    #The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    #Replace with Model ID
    model="seedream-4-0-250828", 
    prompt="Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    size="2K",
    sequential_image_generation="auto",
    sequential_image_generation_options=SequentialImageGenerationOptions(max_images=4),
    optimize_prompt_options=OptimizePromptOptions(mode="fast"),
    response_format="url",
    watermark=False
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Java" key="HJlrQ042GU"><RenderMd content={`\`\`\`Java
package com.ark.sample;

import com.byteplus.ark.runtime.model.images.generation.GenerateImagesRequest;
import com.byteplus.ark.runtime.model.images.generation.ImagesResponse;
import com.byteplus.ark.runtime.model.images.generation.ResponseFormat;
import com.byteplus.ark.runtime.model.images.generation.Size;
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
                .baseUrl("https://ark.ap-southeast.bytepluses.com/api/v3") //The base URL for model invocation
                .dispatcher(dispatcher)
                .connectionPool(connectionPool)
                .apiKey(apiKey)
                .build();
        
        GenerateImagesRequest.SequentialImageGenerationOptions sequentialImageGenerationOptions = new GenerateImagesRequest.SequentialImageGenerationOptions();
        sequentialImageGenerationOptions.setMaxImages(4);
        GenerateImagesRequest.OptimizePromptOptions optimizePromptOptions = new GenerateImagesRequest.OptimizePromptOptions();
        optimizePromptOptions.setMode("fast");
        
        GenerateImagesRequest generateRequest = GenerateImagesRequest.builder()
                 .model("seedream-4-0-250828")  //Replace with Model ID
                  .prompt("Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.")
                 .responseFormat(ResponseFormat.Url)
                 .size("2K")
                 .sequentialImageGeneration("auto")
                 .sequentialImageGenerationOptions(sequentialImageGenerationOptions)
                 .optimizePromptOptions(optimizePromptOptions)
                 .stream(false)
                 .watermark(false)
                 .build();
        ImagesResponse imagesResponse = service.generateImages(generateRequest);
        // Iterate through all image data
        if (imagesResponse != null && imagesResponse.getData() != null) {
            for (int i = 0; i < imagesResponse.getData().size(); i++) {
                // Retrieve image information
                String url = imagesResponse.getData().get(i).getUrl();
                String size = imagesResponse.getData().get(i).getSize();
                System.out.printf("Image %d:%n", i + 1);
                System.out.printf("  URL: %s%n", url);
                System.out.printf("  Size: %s%n", size);
                System.out.println();
            }


            service.shutdownExecutor();
        }
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="Go" key="NQsC1kCBZM"><RenderMd content={`\`\`\`Go
package main

import (
    "context"
    "fmt"
    "os"
    "strings"
    
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/service/arkruntime/model"
    "github.com/byteplus-sdk/byteplus-go-sdk-v2/byteplus"
)

func main() {
    client := arkruntime.NewClientWithApiKey(
        os.Getenv("ARK_API_KEY"),
        // The base URL for model invocation .
        arkruntime.WithBaseUrl("https://ark.ap-southeast.bytepluses.com/api/v3"),
    )    
    ctx := context.Background()
    
    var (
    sequentialImageGeneration model.SequentialImageGeneration = "auto"
    maxImages = 4
    mode model.OptimizePromptMode = model.OptimizePromptModeFast
    )
    
    generateReq := model.GenerateImagesRequest{
       Model:          "seedream-4-0-250828",
       Prompt:         "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
       Size:           byteplus.String("2K"),
       ResponseFormat: byteplus.String(model.GenerateImagesResponseFormatURL),
       Watermark:      byteplus.Bool(false),
       SequentialImageGeneration: &sequentialImageGeneration,
       SequentialImageGenerationOptions: &model.SequentialImageGenerationOptions{
          MaxImages: &maxImages,
       },
       OptimizePromptOptions: &model.OptimizePromptOptions{
       Mode: &mode,
       },
    }

    resp, err := client.GenerateImages(ctx, generateReq)
    if err != nil {
        fmt.Printf("call GenerateImages error: %v\\n", err)
        return
    }

    if resp.Error != nil {
        fmt.Printf("API returned error: %s - %s\\n", resp.Error.Code, resp.Error.Message)
        return
    }

    // Output the generated image information
    fmt.Printf("Generated %d images:\\n", len(resp.Data))
    for i, image := range resp.Data {
        var url string
        if image.Url != nil {
            url = *image.Url
        } else {
            url = "N/A"
        }
        fmt.Printf("Image %d: Size: %s, URL: %s\\n", i+1, image.Size, url)
    }
}
\`\`\`


`}></RenderMd></Tabs.TabPane>
<Tabs.TabPane title="OpenAI" key="eM0p4JQV5o"><RenderMd content={`\`\`\`Python
import os
from openai import OpenAI

client = OpenAI( 
    #The base URL for model invocation
    base_url="https://ark.ap-southeast.bytepluses.com/api/v3", 
    # Get API Key：https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.getenv('ARK_API_KEY'), 
) 
 
imagesResponse = client.images.generate( 
    model="seedream-4-0-250828",
    prompt="Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.",
    size="2K",
    response_format="url",
    extra_body={
        "watermark": false,
        "sequential_image_generation": "auto",
        "sequential_image_generation_options": {
            "max_images": 4
        },
        "optimize_prompt_options": {"mode": "fast"}
    },
) 
 
# Iterate through all image data
for image in imagesResponse.data:
    # Output the current image's URL and size
    print(f"URL: {image.url}, Size: {image.size}")
\`\`\`


* You may replace the Model ID as needed. For Model ID lookup, see [Model List](/docs/ModelArk/1330310).
`}></RenderMd></Tabs.TabPane></Tabs>);
 ```

<span id="3fa0345d"></span>
# Customize Image Output
You can configure the following parameters to control image output specifications:

* **size**: The dimensions of the output image.
* **response_format**: The format of the generated image.
* **watermark**: Whether to add a watermark to the output image.

<span id="034e4a46"></span>
## Image output dimensions
The following methods are available. The two methods cannot be used at the same time.

* Method 1 : Specify the resolution of the generated image, and describe its aspect ratio, shape, or purpose in the prompt using natural language. The model determines the width and height.
   * Optional values: `1K` (Seedream 4.5 not supported), `2K`, `4K`
* Method 2: Specify the width and height of the generated image in pixels.
   * Default value: `2048x2048`
   * Total pixel range:
      * seedream 4.5：[`2560x1440=3686400`, `4096x4096=16777216`]
      * seedream 4.0：[`1280x720=921600`, `4096x4096=16777216`]
   * Aspect ratio range: [1/16, 16]


---



<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

Method 1


</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

Method 2


</div>
</div>


<div style="display: flex;">
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);">

```JSON
{
    "prompt": "Generate a series of 4 posters focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.", // In the prompt, use natural language to describe the aspect ratio, shape, or purpose of the image
    "size": "2K"  // Specify the resolution of the generated image via the size parameter
}
```



</div>
<div style="flex-shrink: 0;width: calc((100% - 16px) * 0.5000);margin-left: 16px;">

```JSON
{
    "prompt": "Generate a series of 4 coherent illustrations focusing on the same corner of a courtyard across the four seasons, presented in a unified style that captures the unique colors, elements, and atmosphere of each season.", 
    "size": "2048x2048"  // Specify the width and height of the generated image in pixels
}
```



</div>
</div>

<span id="b4306703"></span>
## Image output methods
The generated image is in JPEG and can be returned in the following two ways:

* `url`: Return a download link of the image. 
* `b64_json`: Return the image data in JSON as a Base64-encoded string.

```JSON
{
    "response_format": "url"
}
```

<span id="6be7edc7"></span>
## Add a watermark to the image
Control whether to add a watermark to the generated image by setting the **watermark** parameter.

* `false`: No watermark.
* `true`: Add an "AI generated" watermark on the bottom-right corner of the image.

```JSON
{
    "watermark": true
}
```

<span id="31037d05"></span>
# Usage Limitations
**SDK version upgrade**
To ensure model functionalities, upgrade to the latest SDK version. Refer to [Install and upgrade SDK](/docs/ModelArk/1541595) for details.

**Image input limitations**

* Image format: JPEG, PNG, WEBP, BMP, TIFF and GIF
* Aspect ratio (width/height): Between [1/16, 16]
* Width and height (px): Greater than 14 px
* Size: up to 10 MB
* Total pixels: No more than `6000x6000=36000000` px （The total pixel limit applies to the product of the single image’s width and height, rather than to either dimension individually.）
* Reference images: Up to 14 images can be uploaded.


**Retention period**
Task data (such as task status, image URL, etc.) is retained for 24 hours and will be automatically cleared after expiration. Be sure to save your generated images in time.

**Rate limits information**

* RPM rate limit: The maximum number of pictures that can be generated per minute by a specific version of a model under an account. If the limit is exceeded, an error will occur.
* The limit values vary by model. For more details, see [Image generation](/docs/ModelArk/1330310#d3e5e0eb).
