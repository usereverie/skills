Seedance 2.0 series tutorial
Seedance 2.0 series tutorial
Last updated:  April 14, 2026 20:44:27First published:  April 14, 2026 12:14:41
Copy Page

The seedance 2.0 series models (including seedance 2.0 and seedance 2.0 fast) support multimodal input such as images, videos, audios and texts, with capabilities including video generation, video editing, and video extension. They can restore item details, timbres, effects, styles, camera movements and more with high accuracy, maintain consistent character features, and give users director-level control. This topic introduces the exclusive capabilities of the seedance 2.0 series models to help you get started quickly.

tip

Please purchase the prepaid resource pack in advance, otherwise you cannot activate the seedance 2.0 and seedance 2.0 fast models.

Getting started

This getting started tutorial is designed specifically for users new to API, to help you set up a Python development environment, create virtual environments, and install the ModelArk SDK with one click. With the provided out-of-the-box seedance 2.0 code samples, you only need to replace the input assets to start your video creation.


1. Prerequisites

Before you start, make sure you have completed the following preparations:

Register an account: Make sure you have a BytePlus account and are signed in.
Get API Key: Visit the API keys page, click Create API Key, then copy and save your API Key. Make sure to keep your API Key safe and do not disclose it to others.
Activate the models: Please purchase the prepaid resource pack in advance, otherwise you cannot activate the seedance 2.0 and seedance 2.0 fast models.
Download and unzip the file: Click to download the attachment below, and unzip it to your local directory (such as the desktop or "Downloads" folder).

modelark_seedance2.0_quickstart_package.zip
Unknown size

2. Procedure
Windows users
macOS users
Go to the scripts/init_dev_env directory.
Double-click to run setup_windows.bat.
The script will automatically perform the following operations:
Download the uv tool.
Automatically download Python 3.12 (if it does not interfere with the Python installed in your system).
Create the .venv virtual environment.
Install the ModelArk SDK.
After completion, a run_demo.bat file will be generated in the project root directory.
Double-click run_demo.bat to run the Python SDK code sample (python/demo_standard.py).

Open the terminal and go to the scripts/init_dev_env directory.

Run the set up script:

./setup_mac.sh

Plain

The script will automatically configure all environments.

After completion, a run_demo.sh file will be generated in the project root directory.

Run ./run_demo.sh to run the Python SDK code sample (python/demo_standard.py).

3. What the script does

After running the script, you will see the following process:

Verify the API Key: The script will automatically detect whether the ARK_API_KEY environment variable is configured locally. If not, you will be prompted to enter it manually.
Preview the assets: The script will automatically pop up a locally generated HTML page in your default browser, displaying the text prompt for this task, the reference image to be replaced, and the reference video.
Create the task and query for status: The script initiates an asynchronous request to the ModelArk server. Since video generation takes some time, the console will print the task status (such as running) every 30 seconds.
Get the results: After the task is completed successfully, the console will output the URL of the generated video. You can copy the link to your browser to download or play it online.

4. Next steps

After you successfully run this sample, you can try to modify python/demo_standard.py to create your own video-generation task:

Modify the text prompt.
Find the user_content variable in the code and change it to any description you want.
Replace input assets (images, videos, audios).
You can replace reference_image_url, reference_video_url and reference_audio_url with your own asset links. Note: Please make sure the URL is a publicly accessible link on the public network (it is recommended to store it in BytePlus TOS object storage service and configure it for public read access).
Continue to explore the following examples.

Model capabilities

Seedance 2.0 fast has the same model capabilities as seedance 2.0. For the highest generation quality, we recommend using seedance 2.0. If you prioritize cost and generation speed over extreme quality, we recommend using seedance 2.0 fast.

Model Name

		

seedance 2.0

	

seedance 2.0 fast




Model ID

		

dreamina-seedance-2-0-260128

	

dreamina-seedance-2-0-fast-260128




Text-to-video

		

	




Image-to-video (first frame)

		

	




Image-to-video (first and last frames)

		

	




Multimodal-reference-to-video [New]

	

Image reference

	

	




Video reference

	

	




Combined reference

Image + audio
Image + video
Video + audio
Image + video + audio
	

	




Edit video [New]

		

	




Extend video [New]

		

	




Generate video with audio

		

	




Draft mode

		

	




Return video last frame

		

	




Output video specifications

	

Resolution

	

480p, 720p, 1080p

	

480p, 720p


	

Aspect ratio

	

21:9, 16:9, 4:3, 1:1, 3:4, 9:16


	

Duration

	

4–15 seconds

	

4–15 seconds


	

Video format

	

mp4

	

mp4




Offline inference

		

	




Online inference rate limits

	

Maximum RPM

	

Enterprise users: 600 Individual users: 180

	

Enterprise users: 600 Individual users: 180


	

Maximum concurrency

	

Enterprise users: 10
Individual users: 3

	

Enterprise users: 10
Individual users: 3




Offline inference rate limits

	

Maximum TPD

	

-

	

-

Basic usage

Multimodal reference

Input text, reference images, videos (with or without audio tracks) and audios to generate a new video. It can inherit core information including character image, visual style and screen composition from reference images, subject, camera movement, action performance and overall style from reference videos, as well as timbre, music melody and dialogue content from reference audios.
The following are some demos (visit the model card to view more samples):

Input: text

	

Input: image, video, audio

	

Output




Use the first-person POV framing from [Video 1] throughout, and use [Audio 1] as the background music throughout. First-person POV fruit tea promotional ad, seedance limited-edition apple fruit tea; opening frame is [Image 1], your hand picks a dew-covered Aksu red apple, a light, crisp apple tapping sound; 2–4 seconds: fast cuts, your hand drops apple chunks into a shaker, adds ice and tea base, shakes forcefully, ice clinking and shaking sounds sync with upbeat rhythmic beats, background audio: {Fresh-cut, shaken fresh}; 4–6 seconds: first-person close-up of the finished drink, layered fruit tea is poured into a clear cup, your hand gently squeezes milk foam to spread across the top, a pink brand sticker is applied to the cup, the camera moves closer to show the layered textures of the foam and fruit tea; 6–8 seconds: first-person hand-held toast shot, you raise the fruit tea from [Image 2] toward the camera (simulating handing it to the viewer), the cup label is clearly visible, background audio {Take a sip of fresh refreshment}, the final frame freezes on Image 2. All background voice audio uses a female voice.

	
r2v_tea_audio1.mp3
Unknown size

	
Python
import os
import time
# Install SDK:pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    #The base URL for model invocation
    base_url='https://ark.ap-southeast.bytepluses.com/api/v3',
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.environ.get("ARK_API_KEY"),
)

if __name__ == "__main__":
    print("----- create request -----")
    create_result = client.content_generation.tasks.create(
        model="dreamina-seedance-2-0-260128", #Replace with Model ID 
        content=[
            {
                "type": "text",
                "text": "Use the first-person POV framing from Video 1 throughout, and use Audio 1 as the background music throughout. First-person POV fruit tea promotional ad, seedance limited-edition apple fruit tea; opening frame is Image 1, your hand picks a dew-covered Aksu red apple, a light, crisp apple tapping sound; 2–4 seconds: fast cuts, your hand drops apple chunks into a shaker, adds ice and tea base, shakes forcefully, ice clinking and shaking sounds sync with upbeat rhythmic beats, background audio: {Fresh-cut, shaken fresh}; 4–6 seconds: first-person close-up of the finished drink, layered fruit tea is poured into a clear cup, your hand gently squeezes milk foam to spread across the top, a pink brand sticker is applied to the cup, the camera moves closer to show the layered textures of the foam and fruit tea; 6–8 seconds: first-person hand-held toast shot, you raise the fruit tea from Image 2 toward the camera (simulating handing it to the viewer), the cup label is clearly visible, background audio {Take a sip of fresh refreshment}, the final frame freezes on Image 2. All background voice audio uses a female voice.",                
            },
            {
                "type": "image_url",
                "image_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/r2v_tea_pic1.jpg"
                },
                "role": "reference_image",
            },
            {
                "type": "image_url",
                "image_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/r2v_tea_pic2.jpg"
                },
                "role": "reference_image",
            },
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/r2v_tea_video1.mp4"
                },
                "role": "reference_video",
            },
            {
                "type": "audio_url",
                "audio_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_audio/r2v_tea_audio1.mp3"
                },
                "role": "reference_audio",
            },
        ],
        generate_audio=True,
        ratio="16:9",
        duration=11,
        watermark=True,
    )
    print(create_result)


    # Polling query section
    print("----- polling task status -----")
    task_id = create_result.id
    while True:
        get_result = client.content_generation.tasks.get(task_id=task_id)
        status = get_result.status
        if status == "succeeded":
            print("----- task succeeded -----")
            print(get_result)
            break
        elif status == "failed":
            print("----- task failed -----")
            print(f"Error: {get_result.error}")
            break
        else:
            print(f"Current status: {status}, Retrying after 30 seconds...")
            time.sleep(30)

Python

tip

You can combine the following modal content as needed. Note that "text + audio" and "audio-only" inputs are not supported.
Text
Images: 0–9 images
Videos: 0–3 videos
Audio: 0–3 audios
Advanced usage: For multimodal video generation, you can specify reference images as the first/last frame via prompts to indirectly achieve the effect of "first and last frames + multimodal reference". If you need to strictly ensure that the first and last frames are consistent with the specified images, please always use image-to-video (first and last frame) feature (configure the role parameter to first_frame / last_frame).
See Multimodal input for input requirements for each modal information.

Edit video

You can provide the video to be edited, reference images or audio, and use prompts together to complete various video editing tasks, such as replacing the video subject, adding, deleting and modifying objects in the video, redrawing/repairing partial frames, etc.
The following are some demos (visit the model card to view more samples):

Input: text

	

Input: video & image

	

Output




Replace the cat in [Video 1] with the lion from [Image 1]. The lion lies on its side across the girl’s legs, gently interacting with her in a warm and tender way.

	

	
Python
import os
import time
# Install SDK:pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    #The base URL for model invocation
    base_url='https://ark.ap-southeast.bytepluses.com/api/v3',
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.environ.get("ARK_API_KEY"),
)

if __name__ == "__main__":
    print("----- create request -----")
    create_result = client.content_generation.tasks.create(
        model="dreamina-seedance-2-0-260128", #Replace with Model ID 
        content=[
            {
                "type": "text",
                "text": "Replace the cat in [Video1] with the lion from [Image1]. The lion lies on its side across the girl’s legs, gently interacting with her in a warm and tender way.",
            },
            {
                "type": "image_url",
                "image_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/r2v_edit_pic1.jpg"
                },
                "role": "reference_image",
            },
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/r2v_edit_video1.mp4"
                },
                "role": "reference_video",
            },
        ],
        generate_audio=True,
        ratio="16:9",
        duration=12,
        watermark=True,
    )
    print(create_result)


    # Polling query section
    print("----- polling task status -----")
    task_id = create_result.id
    while True:
        get_result = client.content_generation.tasks.get(task_id=task_id)
        status = get_result.status
        if status == "succeeded":
            print("----- task succeeded -----")
            print(get_result)
            break
        elif status == "failed":
            print("----- task failed -----")
            print(f"Error: {get_result.error}")
            break
        else:
            print(f"Current status: {status}, Retrying after 30 seconds...")
            time.sleep(30)

Python

ython Cancel automatic line wrappingCopy

Extend video

Based on the original video, you can extend the video forward or backward, or stitch multiple video clips (up to 3 clips) into a coherent video.
The following are some demos (visit the model card to view more samples):

Input: text

	

Input: video to be extended

	

Output




The arched window in [video 1] opens, and the camera moves into the interior of the art museum, transitioning into [video 2]. After that, the camera enters the painting itself, transitioning into [video 3].

	
	
Python
import os
import time
# Install SDK:pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    #The base URL for model invocation
    base_url='https://ark.ap-southeast.bytepluses.com/api/v3',
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.environ.get("ARK_API_KEY"),
)

if __name__ == "__main__":
    print("----- create request -----")
    create_result = client.content_generation.tasks.create(
        model="dreamina-seedance-2-0-260128", #Replace with Model ID 
        content=[
            {
                "type": "text",
                "text": "The arched window in [video 1] opens, and the camera moves into the interior of the art museum, transitioning into [video 2]. After that, the camera enters the painting itself, transitioning into [video 3].",
                
            },
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/r2v_extend_video1.mp4"
                },
                "role": "reference_video",
            },
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/r2v_extend_video2.mp4"
                },
                "role": "reference_video",
            },
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/r2v_extend_video3.mp4"
                },
                "role": "reference_video",
            },
        ],
        generate_audio=True,
        ratio="16:9",
        duration=8,
        watermark=True,
    )
    print(create_result)


    # Polling query section
    print("----- polling task status -----")
    task_id = create_result.id
    while True:
        get_result = client.content_generation.tasks.get(task_id=task_id)
        status = get_result.status
        if status == "succeeded":
            print("----- task succeeded -----")
            print(get_result)
            break
        elif status == "failed":
            print("----- task failed -----")
            print(f"Error: {get_result.error}")
            break
        else:
            print(f"Current status: {status}, Retrying after 30 seconds...")
            time.sleep(30)

Python

tip

When you extend a video clip forward or backward, the generated video usually only includes the tail footage of the original video. But you can also flexibly control it via a prompt to make it include the original video content. For example: Extend Video 1 backward, [description of the extended content...], and connect to Video 1 at the end.
When you pass 2 to 3 video clips to fill in the intermediate transition part, the generated video will include both the original video content and the newly generated video content.

More capabilities

seedance 2.0 series models also support common basic capabilities such as text-to-video, first-frame image-to-video, first-and-last-frame image-to-video, and video output specification configuration. For details, see Video generation tutorial.


Create with ease

Seedance 2.0 series models do not support direct upload of reference images or videos containing real human faces. The following solutions are provided to make it easier for creatives to use portraits.

Solution

	

Overview




Digital characters

	

ModelArk has a pre-built digital character library that provides creatives with free, compliant, and diverse portrait assets. It is suitable for scenarios that require realistic but not specific human faces, and pursue zero compliance risk and fast creation.




Authorized real-person assets

	

Supports video generation using authorized real portrait assets.




Authorized real-person assets

	

Videos generated by seedance 2.0/2.0 fast models under your account can be directly used for video editing or extension without being intercepted by moderation.

Digital characters

For realistic style videos, you can control the character appearance through pre-built avatars in the digital character library. Each asset has a unique asset ID. You can generate a video by passing asset://<asset ID> in the content._url.url parameter. To browse and search for digital characters, see Digital character library.

Input: text

	

Input: digital character, image

	

Output




Vertical HD close-up video of a beauty blogger (Image 1). She has bold, glamorous makeup with no facial shine or glare and a sweet smile. She holds a face cream jar (Image 2), presents it directly to the camera. The background is fresh and minimalist. Energetic and sweet style. Character speaks in real-time: 'I found my holy grail face cream! It has a cloud-like creamy texture that absorbs instantly. Perfect for post-all-nighter rescue, deep hydration and moisturization—my skin glows naturally even without makeup!'"

warning

Asset ID is only used to pass assets to the model. In the prompt, you still need to reference assets in the format "asset type+number", where the number is the sorting order of the asset among assets of the same type in the request body.
Correct usage: Beauty influencer in image 1
Incorrect usage: asset-2026**** is a beauty influencer

	

Digital character

Product image

	
Python
import os
import time
# Install SDK:pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    #The base URL for model invocation
    base_url='https://ark.ap-southeast.bytepluses.com/api/v3',
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.environ.get("ARK_API_KEY"),
)

if __name__ == "__main__":
    print("----- create request -----")
    create_result = client.content_generation.tasks.create(
        model="dreamina-seedance-2-0-260128", #Replace with Model ID 
        content=[
            {
                "type": "text",
                "text": "Vertical HD close-up video of a beauty blogger (Image 1). She has bold, glamorous makeup with no facial shine or glare and a sweet smile. She holds a face cream jar (Image 2), presents it directly to the camera. The background is fresh and minimalist. Energetic and sweet style. Character speaks in real-time: 'I found my holy grail face cream! It has a cloud-like creamy texture that absorbs instantly. Perfect for post-all-nighter rescue, deep hydration and moisturization—my skin glows naturally even without makeup!'"
            },        
            {
                "type": "image_url",
                "image_url": {
                    "url": "asset://asset-20260410114236-8cdfz"
                },
                "role": "reference_image"
            },
            {
                "type": "image_url",
                "image_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_image/r2v_edit_pic1.jpg"
                },
                "role": "reference_image"
            },
        ],
        generate_audio=True,
        ratio="adaptive",
        duration=11,
        watermark=True,
    )
    print(create_result)

    print("----- polling task status -----")
    task_id = create_result.id
    while True:
        get_result = client.content_generation.tasks.get(task_id=task_id)
        status = get_result.status
        if status == "succeeded":
            print("----- task succeeded -----")
            print(get_result)
            break
        elif status == "failed":
            print("----- task failed -----")
            print(f"Error: {get_result.error}")
            break
        else:
            print(f"Current status: {status}, Retrying after 30 seconds...")
            time.sleep(30)

Python

Authorized real-person assets

After passing real-person verification and obtaining personal authorization, you can upload relevant assets of the real person (such as images, videos, and audio of the real person) to ModelArk. After the asset is successfully registered, each asset will get an independent Asset ID. You can pass asset://<asset ID> in the content._url.url parameter to use this asset to generate videos. For the real-person verification and asset registration process, see Add real-human assets to asset library.

...
"content": [
         {
            "type": "text",
            "text": "<your prompt>"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "asset://<asset ID>"
            },
            "role": "reference_image"
        },
        {
            "type": "video_url",
            "video_url": {
                "url": "asset://<asset ID>"
            },
            "role": "reference_video"
        },
        {
            "type": "audio_url",
            "audio_url": {
                "url": "asset://<asset ID>"
            },
            "role": "reference_audio"
        }
    ]
...

JSON

Derivative works using model outputs

Seedance 2.0 series models do not support direct upload of reference images or videos containing real human faces. However, ModelArk trusts face-containing videos generated by seedance 2.0 and 2.0 fast models. You can use the original face-containing videos generated by the above models under your account within the past 30 days as input assets to generate, edit, or extend videos with seedance 2.0 series models.

warning

For face-containing use cases, ModelArk only trusts original videos generated by seedance 2.0 series models within the last 30 days. Modified or expired videos cannot be used.
For use cases that do not use human faces, there is no trust issue with model outputs, and you can create or modify as needed.

Input: video generated under the same account

	

Output




use the example from Digital characters

	

Input: Change the color of the cream to white.
Change the ratio to 16:9.

Generate an initial video, and obtain the video URL. Here we directly use the example from Digital characters.
Edit the video generated by seedance 2.0 again. The original video URL is only valid for 24 hours. In this example, the original video is transferred to BytePlus TOS for use.

tip

The original video URL is only valid for 24 hours. For actual use, it is recommended that you transfer and save the video file in advance. It is recommended to configure the data subscription function provided by BytePlus TOS to automatically transfer your video outputs to your own TOS bucket for long-term backup or further processing. For details, see Data subscription.

Python
import os
import time
# Install SDK:  pip install byteplus-python-sdk-v2 
from byteplussdkarkruntime import Ark 

client = Ark(
    # The base URL for model invocation
    base_url='https://ark.ap-southeast.bytepluses.com/api/v3',
    # Get API Key: https://console.byteplus.com/ark/region:ark+ap-southeast-1/apikey
    api_key=os.environ.get("ARK_API_KEY"),
)

if __name__ == "__main__":
    print("----- create request -----")
    create_result = client.content_generation.tasks.create(
        model="dreamina-seedance-2-0-260128", # Replace with Model ID 
        content=[
            {
                "type": "text",
                "text": "Change the color of the cream to white."
            },                
            {
                "type": "video_url",
                "video_url": {
                    "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/doc_video/video_by_sd2.mp4"
                },
                "role": "reference_video"
            },
        ],
        generate_audio=True,
        ratio="16:9",
        duration=11,
        watermark=True,
    )
    print(create_result)
    print("----- polling task status -----")
    task_id = create_result.id
    while True:
        get_result = client.content_generation.tasks.get(task_id=task_id)
        status = get_result.status
        if status == "succeeded":
            print("----- task succeeded -----")
            print(get_result)
            break
        elif status == "failed":
            print("----- task failed -----")
            print(f"Error: {get_result.error}")
            break
        else:
            print(f"Current status: {status}, Retrying after 30 seconds...")
            time.sleep(30)

Python

Prompt engineering techniques

Prompts must reference assets in the format asset type + number, where the number is the sorting order of the asset among assets of the same type in the request body. For example, "Image n" refers to the nth reference image with type="image_url" in the content array (counting starts from 1 in array order).Note that referencing assets by Asset ID is not supported.
The following section describes typical prompt formulas for multimodal reference, video editing, and video extension. For more details, see Seedance 2.0 & 2.0 fast prompt guide.

tip

ModelArk provides the seedance 2.0 prompt optimization skill to help you tune your prompts.

How to install: You can configure the skill file in Code Agent / AI Agent to use it. Take OpenClaw as an example: download the SKILL.md file, copy the full content to the dialog input box, send "Please install this skill", and wait for the tool to complete the installation automatically.
How to use: Enter /sd2-pe + your prompt content in the AI dialog box to start debugging prompts.

SKILL.md
Unknown size

Multimodal reference
Image reference: Reference / extract / combine + "subject / referenced element description" from "Image n" to generate "plot description", keeping the characteristics of "subject / referenced element description" consistent.
Video reference: Reference "action description / camera movement description / special effect description" from "Video n" to generate "plot description", keeping the action details / camera movement / special effects consistent.
Audio reference:
Voice timbre reference: "Character" says: "lines", voice timbre references "Audio n".
Audio content reference: Ideal appearance timing + "Audio n".

Edit video
Add elements: Clearly describe "element characteristics" + "appearance timing" + "appearance position".
Delete elements: Specify the elements to be deleted, and emphasize the elements that remain unchanged in the prompt for better result.
Modify elements: Simply clearly describe the elements to be replaced.

Extend video
Extend video: Extend "Video n" forward/backward + "description of the video to be extended".
Track completion: "Video 1" + "transition description" + connect to "Video 2" + "transition description" + connect to "Video 3".

Limitations

See Limitations.

Previous documentation
Seededit 3.0 Tutorial
Next documentation
Video generation tutorial
Getting started
1. Prerequisites
2. Procedure
3. What the script does
4. Next steps
Model capabilities
Basic usage
Multimodal reference
Edit video
Extend video
More capabilities
Create with ease
Digital characters
Authorized real-person assets
Derivative works using model outputs
Prompt engineering techniques
Multimodal reference
Edit video
Extend video
Limitations
