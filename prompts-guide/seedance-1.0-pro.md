This article introduces prompt tips for text-to-video and image-to-video functions of Seedance-1.0-pro and Seedance-1.0-pro-fast. It helps you quickly get started with video creation and turn ideas into video content.
<span id="8489a3ee"></span>
## Model Introduction
**Seedance 1.0**  is a series of basic video generation models newly launched by the ByteDance Doubao Large Model Team. 

* **Seedance 1.0 Pro** , as the large-parameter version of this model series, has unique multi-shot narrative capability and performs excellently in all dimensions. It has made breakthroughs in semantic understanding and instruction following capabilities, capable of generating 1080P high-definition videos with smooth movement, rich details, diverse styles, and cinematic aesthetics.
* **Seedance 1.0 Pro Fast** is a full-scale model at rock bottom and top performance, achieving an excellent balance between video production quality, speed, and cost. Based on the core advantages of the Seedance 1.0 Pro model, the generation speed is faster than that of Seedance 1.0 Pro is up to 3 times higher, and the price is highly competitive, bringing creators an experience optimized for efficiency and cost.


<span id="7fda8bcc"></span>
## Prompt parameters
In [Create video generation task API](https://docs.byteplus.com/docs/ModelArk/1520757#bb804461), the parameters related to the prompt are as follows:
**Content.text**：Text input to the model that describes the expected generated video, including:

   * **Prompts (required)**: Support Chinese and English.
   * **Model text command (optional)**: Append -- [parameters] after the text prompt to control the specifications of the video output. This article mainly uses:
      * Resolution `abbreviated as rs`: Resolution
      * Duration `abbreviated as dur`: Generate video duration (seconds) 
      * CameraFixed `abbreviated as cf`: Whether to fix the camera


```Plain Text
{
    "model": "seedance-1-0-pro-250528",
    "content": [
        {
            "type": "text",
            "text": "At breakneck speed, drones thread through intricate obstacles or stunning natural wonders, delivering an immersive, heart-pounding flying experience.  --resolution 1080p  --duration 5 --camerafixed false"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "https://ark-doc.tos-ap-southeast-1.bytepluses.com/seepro_i2v%20.png"
            }
        }
    ]
}
```

<span id="481f1c04"></span>
## Action Prompt
<span id="39d4f801"></span>
### Fundamental Actions
> Beginner：subject+action


| || \
|Video Generation Example | |
|---|---|
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4e751a0214e04b719c43f6169c681a17~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4e751a0214e04b719c43f6169c681a17~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|t2v：The kitten yawns at the camera. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d48bbf1125284ae2b59a480b521affb7~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d48bbf1125284ae2b59a480b521affb7~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |t2v：A woman is walking on the streets of Shanghai at night. |
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/498fd3308d8b48b1b46a7f0db9e515a0~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/498fd3308d8b48b1b46a7f0db9e515a0~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|i2v：The man turns his head and smiles at the camera. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ed126865e0684f729b8144aae17ff59f~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ed126865e0684f729b8144aae17ff59f~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |i2v：The steady and indifferent boy looks at the camera and takes off his earphones. Then he jumps off the tire, walks toward the camera, and squats down. |

<span id="44f039bf"></span>
### Multi-Action Prompt
> Advanced Gameplayer：Describe multiple actions clearly according to the timeline of their occurrence, to achieve single-character multi-action or multi-character multi-action.


| ||| \
|Video Generation Example | | |
|---|---|---|
| | | | \
|**single-character multi-action** |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/22c1e58cc12b4b3eb02a011c7f24df6c~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/22c1e58cc12b4b3eb02a011c7f24df6c~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|i2v：The woman picks up the wine glass in front of her, takes a sip, puts it down, then stands up and leaves her seat. |**multi-character multi-action** |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4fe10b493b3f4844ac558b3307cba236~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4fe10b493b3f4844ac558b3307cba236~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |t2v：At a rock band's live performance, the lead singer holds a microphone and sings on stage, the guitarist plays the guitar energetically, the bassist plays the bass, the drummer bangs the drums while shaking their head, and the keyboardist plays the piano. |**multi-character multi-action** |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/22ce7555762c4c88a826a2e8d69adf5c~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/22ce7555762c4c88a826a2e8d69adf5c~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |t2v：In the office pantry, colleagues are taking a break and chatting. Colleague A shares weekend fun stories with animated gestures, waving their hands and gesturing excitedly. Colleague B is laughing so hard that they are doubling over, while Colleague C curiously presses for details. The others gather around, chiming in with a few words from time to time. |

<span id="a395fd3b"></span>
## Lens Language
<span id="a6a1df1c"></span>
### Basic Camera Movements
> The Seedance 1.0 Pro version can accurately respond to camera movement prompt, such as **Tracking Shot、Pan Left /Right、Truck Left/Right.**


| ||| \
|Video generation example | | |
|---|---|---|
| | | | \
|Push |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0cee88af61354f2fa266c0baa73b46cc~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/0cee88af61354f2fa266c0baa73b46cc~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|i2v：The camera quickly pushes into a close shot of the little girl, who turns her back to the lens and slowly raises her head to look up at the building in front of her. |Pull |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b801045958534b3db532ec91f8ca4c6c~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b801045958534b3db532ec91f8ca4c6c~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |i2v：The camera quickly pulls out, revealing the woman's upper body. She slightly turns her head and looks to the right of the frame, with a bustling street as the background. |Pan |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c04a39f0172c44f7b0954f448eb2dea2~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c04a39f0172c44f7b0954f448eb2dea2~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |t2v：At a fashion show scene, the camera pans right to shoot a gorgeously dressed model walking the runway in profile. |
| | | | \
|Move |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cbbd0408a95a4fae902fb71d5951c48b~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cbbd0408a95a4fae902fb71d5951c48b~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|i2v：The camera moves right to showcase the grandeur of the Great Wall, captured through miniature photography where the Great Wall is constructed of fabrics. |Circle around |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5a2f262ad1cb4d769dd628097462ae45~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/5a2f262ad1cb4d769dd628097462ae45~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |i2v：The camera circles around, moving from the woman's back to her front. She is extremely beautiful, raising her hand to cover her mouth and smiling shyly. |Follow |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4f1aa9a795f4420596c78f63d2b16bfa~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4f1aa9a795f4420596c78f63d2b16bfa~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |i2v：The lion is flying, and the camera follows the flying lion. |
| | | | \
|Move |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ff2c0b5bdaf7437dbf80c662560973db~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ff2c0b5bdaf7437dbf80c662560973db~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|i2v： The camera moves right, and the woman on the right side of the frame is looking at him affectionately. |\
| |Rise |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a8abe846ef0b430991cab7211a11f611~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a8abe846ef0b430991cab7211a11f611~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |i2v：The head gradually rises, revealing the climber's back. |\
| | |Zoom |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/85dfc5e768ad4d6ca13d17f273a3c84d~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/85dfc5e768ad4d6ca13d17f273a3c84d~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |t2v：In the courtroom, the judge is about to announce the verdict. Under a zoom lens, the camera pushes in on the defendant's nervous face as the courtroom and the crowd in the background gradually pull away, compressing the space to highlight the defendant's suffering while waiting for the judgment. This creates a solemn and tense atmosphere, making the audience's emotions tense as well. |\
| | | |

<span id="47aebb1a"></span>
### Complex Camera Movements
> For advanced players, multiple camera movement commands can be combined to create creative long shots.


| || \
|Video Generation Example | |
|---|---|
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e639f71a903840caa0130e045349bef8~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e639f71a903840caa0130e045349bef8~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
|t2v：A little girl is playing with her puppy on the living room carpet. The camera starts from a ground-level perspective at the puppy's eye line. As the puppy happily runs toward the girl, the camera follows it smoothly, then tilts upward as it approaches the girl to reveal her gentle smile. While the girl and puppy frolic, the camera performs a slow, close-range 360-degree pan around them. Finally, when the girl picks up the puppy, the camera gradually zooms in from below, freezing on their intimate faces. The scene is filled with warmth and affection, presented in a soft visual tone. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a1dc7a7632b543febec49942df4f7ff0~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a1dc7a7632b543febec49942df4f7ff0~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |t2v：A woman stands quietly by a bright window, holding a coffee cup. The camera starts from behind her, slowly pushing forward and sweeping over her right shoulder to delicately capture the steam rising from the coffee and the contours of her serene profile. Without pausing, the camera continues moving forward through the window (or simulates a 穿透 effect), offering a fleeting glimpse of the street scene outside, then smoothly rotates 180 degrees to face the interior again. From the perspective outside the window, it frames the woman's back, finally pulling away slowly. The single-take shot highlights the beauty of light and shadow and the sense of spatial depth. |

<span id="4c5d7afd"></span>
### Control of Shot Sizes and Perspectives
> Professional shot sizes such as **long shot**, **full shot**, **medium shot**, **close-up**, can be controlled by professional description . Specific viewing angles can also be chosen: **underwater shots**, **aerial shots**, **high-angle** , **low-angle** , **macro photography**, **shots with xx as the foreground**, etc.


| || \
|Video Generation Example | |
|---|---|
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c3b3365e29034ed796a1f9f5d8a21089~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c3b3365e29034ed796a1f9f5d8a21089~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：**Macro photograph**y: A caterpillar crawls on a petal, and the hairs on its body can be clearly seen. |\
| |\
| | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/407213a0068342e9a0b26c55258d7a0c~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/407213a0068342e9a0b26c55258d7a0c~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：In the vast desert, a caravan of camels moves slowly. Aerial **shots from high** above capture the contrast between the immense expanse of the desert and the tiny caravan, highlighting the hardship of the journey. |
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f8eb811c165241908f09805e0a76d876~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/f8eb811c165241908f09805e0a76d876~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v: In the living room, a father is teaching his son to play chess. **Over-the-shoulder shots** over the father's shoulder capture the son's thoughtful expression and the chessboard layout, conveying the warm companionship between parent and child. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d998f8cd740c441387b141d973f0cd3a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d998f8cd740c441387b141d973f0cd3a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：Shooting through a box, there are two people looking inside the box. One of them reaches into the box and takes out a small kitten. |

<span id="2f21e646"></span>
## Multi-Stylized Videos 
> Seedance 1.0 pro-t2v has the capability to directly output various styles, including 2D/3D, as well as more subdivided types such as **voxel**, **pixel, felt**, **clay**, **illustration,** etc.


| ||| \
|Video Generation Example | | |
|---|---|---|
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9b34e09d8cc941818fe165ca061de28a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9b34e09d8cc941818fe165ca061de28a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v:In a black-and-white line drawing style, a girl in black-and-white line art walks to the right, with a line drawing forest as the background. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9f54f0f315f14cdf8aab28a67008695a~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9f54f0f315f14cdf8aab28a67008695a~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v:A cute felt kitten is walking on a street made of clay. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1afe09ebf0054daa855752b1bfc4ebd6~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1afe09ebf0054daa855752b1bfc4ebd6~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |t2v:3D cartoon: A personified horse sitting in a classroom attending a class. |
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e4beadc4565b441f83199f2c62ae5812~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/e4beadc4565b441f83199f2c62ae5812~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
| |\
|t2v：Japanese manga: A beautiful woman wearing sunglasses takes a selfie on the streets of Tokyo. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3c795a7d9a344a3cab084765038042f1~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3c795a7d9a344a3cab084765038042f1~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |\
| |t2v：Voxel style, a robot sitting on a rocket. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d0b5ef7bbec14fefa33464de6c630984~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d0b5ef7bbec14fefa33464de6c630984~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | | |\
| | |t2v:American comic style, a muscular man lifting weights. |

<span id="6fe54e4f"></span>
## Prompt Controls the Aesthetic Feeling
<span id="fb485a33"></span>
### Character Appearance
> You can give full play to your imagination, meticulously depict the details of characters, scenes, and clothing, and generate characters with various facial features.


| ||| \
|Video Generation Example | | |
|---|---|---|
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/336670697636439585f727fa473d4987~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/336670697636439585f727fa473d4987~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v: A beautiful Chinese woman, wearing an elegant black cheongsam, sits in a Western-style living room and smokes a cigarette. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c2b337982d8a43c1b41f706d1c0c1c0c~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/c2b337982d8a43c1b41f706d1c0c1c0c~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v: A plain-looking woman wearing a black cheongsam, sitting in a Western-style living room smoking |\
| | | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d7a2c174d40f4c0ea368418f8fee45c2~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d7a2c174d40f4c0ea368418f8fee45c2~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |t2v:The young woman with a slightly chubby face stares at the camera. She has three-white eyes, a mole at the corner of her eye, and rough skin. Her face is illuminated by both red and blue lights. |
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2de1c10d4e224cd78eabcbb95f73bd57~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2de1c10d4e224cd78eabcbb95f73bd57~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：A man with a wretched appearance and messy hair is eating a chicken drumstick, with a bare room as the background. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/17905961d3264752a0c66244c6a89aca~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/17905961d3264752a0c66244c6a89aca~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v: A girl is taking a selfie, making a "Scissors hands" sign at the camera. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96e3a17d79bd4b66b450d18106f571da~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/96e3a17d79bd4b66b450d18106f571da~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |t2v: A 300-pound white man is curled up on the sofa watching TV, with light reflected from the screen rippling across his face. |\
| | | |

<span id="8705806e"></span>
### Visual Aesthetics
> Describe the view in a refined way, use natural language to write out the atmospheric characteristics of the view, and control the overall aesthetic feeling of the view.


1. **Write the video type to control the frame characteristics.**


| || \
|Video Generation Example | |
|---|---|
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bf267a451d894ceaad4fceaa09728ca8~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/bf267a451d894ceaad4fceaa09728ca8~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：The hands of the man and the woman are held together. It's a festive and rustic short video. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3468617a43494000a33cf81296e3b154~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3468617a43494000a33cf81296e3b154~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：The hands of the man and the woman are intertwined. It is an European art-house film. |
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4c2007059f774a7c82e62f895faab407~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/4c2007059f774a7c82e62f895faab407~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：The hands of the man and the woman are clasped together. It is a retro Hong Kong film. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3d1638f0053c40218f7fb8b5f047a145~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3d1638f0053c40218f7fb8b5f047a145~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：The hands of the man and the woman are intertwined. It's a horror movie. |


2. **Describe the desired atmosphere in natural language, which can be either positive or negative, so as to achieve the effect of controlling the aesthetic feeling of the view.**


| || \
|Video generation example | |
|---|---|
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a43cc5bf62c94c2a9cf106bc8c8e0ee2~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/a43cc5bf62c94c2a9cf106bc8c8e0ee2~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v: Oil painting film style scene: in the English countryside, a blonde woman in a knitted sweater and a handsome man share a soulful gaze. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/14961c4d7d2d461bba4baa4e8814e6b4~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/14961c4d7d2d461bba4baa4e8814e6b4~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：A textured old movie with a retro atmosphere: a street musician plays the violin intoxicatedly under the neon lights of a night-time bar. |
| | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d025598ae4344529ac62ae8a45391ddb~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/d025598ae4344529ac62ae8a45391ddb~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：An 1980s TV drama with an old-fashioned and cheap makeup and costume style: a man is writing under a table lamp. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/81470a27f6d246f79311601a8c18bee5~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/81470a27f6d246f79311601a8c18bee5~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：On the leaves of the garden, a group of pixies reside. As the **camera pans right**, the protagonist steps out from home, clad in a petal cloak and holding a grass-leaf wand. At the tip of the wand is an embedded glowing yellow gem, styled in a microcosmic world. |

<span id="d3705525"></span>
## Multi-Lens Capability
> Seedance 1.0 pro supports including multiple scene switches in the same prompt. These scene switches will maintain the continuity of the **subject/style/scene** according to the content of the prompt. Lens changes are connected by "**camera/scene switch**". After each scene is switched, if the scene and characters change, the prompt can be used to depict the characteristics of the newly appeared characters/scenes.


| || | \
|Video generation example | | |
|---|---|---|
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9cb3e6212be24e87a7fbe55ac98ea858~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/9cb3e6212be24e87a7fbe55ac98ea858~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v: |\
|2D animation in American comic style, medium close-up shot of a young and handsome white man who releases his hands, stretches and yawns.  |\
|**Camera switch.** A woman is holding a camera, filming the white man, who crosses his hands and props his arms on his knees.  |\
|**Camera switch.** A top-down shot of a magazine on the table. A hand appears in the lower left corner of the frame, holding a cup of coffee and placing it on the magazine. The coffee is steaming. |\
|Prompt source：Artificial Analysis | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1926cc14b14f4e52b4225792001a8ebf~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1926cc14b14f4e52b4225792001a8ebf~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |i2v: The ship cuts through the storm as lightning repeatedly splits the night sky. **Switch to a medium shot**: a captain stands on the deck, holding a vintage telescope and gazing into the distance. The **camera slowly pushes forward** as he stows the telescope, his expression resolute as he looks ahead. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/45a632c1822341eb94585bc85f930648~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/45a632c1822341eb94585bc85f930648~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |t2v: Push in for a close-up of the red-haired girl's astonished expression. The camera switches to a flowerpot on a windowsill in the ruins, containing a blue succulent plant. The camera switches to an overhead shot as the girl walks toward the succulent. The camera switches to a close-up of the girl's eyes with the succulent in the foreground, then pan to her mouth as she whispers the plant's name. |
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ed6c7b47dcb7451e9840291ffb62faca~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/ed6c7b47dcb7451e9840291ffb62faca~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
| |\
|t2v: In a dilapidated factory, a detective is investigating a bizarre case. The scene begins with a low-angle upward shot to highlight the detective's tall and resolute figure as he slowly walks deep into the factory. Then the **camera pans to follow** him, switching to a level shot to reveal the surrounding cluttered machines and scattered parts. Subsequently, the camera zooms in and shifts to a slightly level shot, focusing on a bloody footprint on the ground to create a tense and suspenseful atmosphere. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/16e8eae2ac6342889ae2f7e537c76a70~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/16e8eae2ac6342889ae2f7e537c76a70~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| | |\
| |t2v: A bizarre scene from a sci-fi movie, in a panoramic shot: inside a future laboratory, the core of the image features a quantum computer, with a scientist beside it continuously operating on a holographic projection screen. The camera switches to a close-up of the quantum computer, which suddenly bursts into red light. Then the camera switches again to the scientist's face, in a close-up low-angle shot, as the red glow shines on his face and his expression starts to turn flustered. | |

<span id="1a350c84"></span>
## Creative Effects
> The model itself can achieve a variety of special effects, and by using imagination, many interesting effects can be realized.


| ||| \
|video generation example | | |
|---|---|---|
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/826f39ab53fa416caef552221bb75a2b~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/826f39ab53fa416caef552221bb75a2b~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|t2v：Under the night lights of New York Harbor, the Statue of Liberty suddenly ejects huge flames and smoke from its base, ascending slowly like a rocket. The flames illuminate the night sky as air currents buffet surrounding buildings and the sea. The camera follows her as she accelerates upward, tracing a bright fiery trajectory. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2da9da273308463abe1b8327a067fab6~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2da9da273308463abe1b8327a067fab6~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |t2v：A chubby bullfrog is sprawled on a pink massage chair, its bulging belly heaving with each breath, and its hands lazily dangling over the armrests. Beside it, a long-haired white cat tiptoes, gently kneading the bullfrog's tense shoulders with its meaty paw pads. The skillful milk-kneading movements make it seem like the most professional massage therapist. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2d0ea2a01998486390de6705958172c9~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/2d0ea2a01998486390de6705958172c9~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |t2v：A tomb robber, now nothing but a skeleton, lies collapsed on the blue bricks of an ancient tomb. His two bony hands drag the skeletal frame as it slowly and laboriously crawls forward, inch by inch. The skull wears a grinning smile. The tomb is dim and fragrant, with candlelight casting shadows on the skeleton's face. Scattered on the ground are fragments of glazed tiles, shards of blue-and-white porcelain bowls, and rusted ancient coins. |
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8c19361a710b46c280255f59c0f4ad49~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/8c19361a710b46c280255f59c0f4ad49~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|i2v：The boy puts down his book, unbuttons his clothes to reveal a Spider-Man bodysuit, dons a Spider-Man mask, shoots mucus off-screen, and quickly flies upward out of the frame toward the top of the lens. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b0486e5bbb754e59b50cb2a7199609be~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/b0486e5bbb754e59b50cb2a7199609be~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |i2v：It's extremely hot. The boy is sweating profusely, with white smoke rising from his body. He starts to melt and flows out of the frame. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1fd4fa8b96224574b96e99dddda807ab~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/1fd4fa8b96224574b96e99dddda807ab~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |i2v：The boy gets angry and pouts his lips. Gradually, his whole body starts to puff up until he explodes, sending many mechanical parts flying out of the frame. |
| | | | \
| |\
|<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7624ae1937f24296915ceaa1bc92fd81~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/7624ae1937f24296915ceaa1bc92fd81~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| |\
|i2v：The boy is reading a book, and as time passes, he slowly grows old. His cheeks sag more and more, the pores on his skin become increasingly visible, sideburns and a beard sprout, transforming him into a weathered uncle. The image also gradually shifts to a grainy black-and-white style. | |\
| |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3ede49671206422ba237e8cd328c7179~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/3ede49671206422ba237e8cd328c7179~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | |\
| |i2v：The boy stares into the camera, instantly feeling like he's fallen in love. His cheeks flush bright red as translucent pink bubbles float in the air, creating an intimate atmosphere. Shyly, he hides his face behind a book—its cover features a heart drawing. | |\
| | |<BytedReactXgplayer config={{ url: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cdbc234a678b4af7a554cfcf7608e095~tplv-goo7wpa0wc-image.image', poster: 'https://p9-arcosite.byteimg.com/tos-cn-i-goo7wpa0wc/cdbc234a678b4af7a554cfcf7608e095~tplv-goo7wpa0wc-video-poster.jpeg' }} ></BytedReactXgplayer> |\
| | | |\
| | |i2v：A violent storm rages. The boy stares wide-eyed at the book, suddenly confused, then quickly understand. At the same time, a bolt of lightning strikes him. When hit by thunder, the boy's entire body appears as if burned, with an exaggerated afro hairstyle and smoke billowing from him. The soot-covered boy, with a face like charred coal, looks up at the camera innocently. |

<span id="e5078c62"></span>
## The Frame Size Following
Seedance 1.0 pro aspect ratios supported by the model include: 1:1，3:4，4:3，16:9，9:16，21:9。
i2v recommends using images with these aspect ratios as the first/last frame. If the images do not conform to these aspect ratios, the automatic matching will adapt them by cropping to the closest applicable ratio.




<style>
/* Override width and height of inline styles */
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default
  .xgplayer-volume-large.xgplayer-pause.xgplayer-is-replay.xgplayer-ended,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-nostart.xgplayer-skin-default.xgplayer-inactive,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-nostart.xgplayer-skin-default,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-volume-large.xgplayer-playing.xgplayer-pause,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-volume-large.xgplayer-playing.xgplayer-inactive,
.volc-md-viewer .editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-volume-large.xgplayer-playing,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-is-enter,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-is-enter.xgplayer-inactive,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-playing,
.editor-video-box.xgplayer.xgplayer-pc.xgplayer-skin-default.xgplayer-volume-large.xgplayer-isloading.xgplayer-playing {
    width: 360px !important;
    height: 180px !important;
}
</style>
