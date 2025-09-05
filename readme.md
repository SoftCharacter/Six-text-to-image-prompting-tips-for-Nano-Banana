Meet Nano Banana (aka Gemini 2.5 Flash Image): our latest, fastest, and most efficient model. Its native multimodal architecture processes text and images in a single step, unlocking powerful capabilities like conversational editing, multi-image composition, and logical reasoning.

You can try these with code from the [official documentation](https://ai.google.dev/gemini-api/docs/image-generation)

or start creating right away in [Google AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemini-2.5-flash-image-preview)

at [ai.studio/banana](https://ai.studio/banana)

Here are the key things you can do:

* Text-to-image: Generate high-quality images from simple or complex text descriptions.
* Image + text-to-image (editing): Provide an image and use text prompts to add, remove, or modify elements, change the style, or adjust colors.
* Multi-image to image (composition & style transfer): Use multiple input images to compose a new scene or transfer the style from one image to another.
* Iterative refinement: Have a conversation to progressively refine your image over multiple turns, making small adjustments until it's perfect.
* Text rendering: Generate images that contain clear and well-placed text, ideal for logos, diagrams, and posters.

This guide will teach you how to write prompts and provide instructions that get the best results from Gemini 2.5 Flash. It all starts with one fundamental principle:

> Describe the scene, don't just list keywords. The model's core strength is its deep language understanding. A narrative, descriptive paragraph will almost always produce a better, more coherent image than a simple list of disconnected words.

# Creating Images from Text

The most common way to generate an image is by describing what you want to see.

## 1. Photorealistic Scenes

For realistic images, think like a photographer. Mentioning camera angles, lens types, lighting, and fine details will guide the model toward a photorealistic result.

> Template: A photorealistic [shot type] of [subject], [action or expression], set in [environment]. The scene is illuminated by [lighting description], creating a [mood] atmosphere. Captured with a [camera/lens details], emphasizing [key textures and details]. The image should be in a [aspect ratio] format.

> Example Prompt: A photorealistic close-up portrait of an elderly Japanese ceramicist with deep, sun-etched wrinkles and a warm, knowing smile. He is carefully inspecting a freshly glazed tea bowl. The setting is his rustic, sun-drenched workshop. The scene is illuminated by soft, golden hour light streaming through a window, highlighting the fine texture of the clay. Captured with an 85mm portrait lens, resulting in a soft, blurred background (bokeh). The overall mood is serene and masterful. Vertical portrait orientation.



```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="A photorealistic close-up portrait of an elderly Japanese ceramicist with deep, sun-etched wrinkles and a warm, knowing smile. He is carefully inspecting a freshly glazed tea bowl. The setting is his rustic, sun-drenched workshop with pottery wheels and shelves of clay pots in the background. The scene is illuminated by soft, golden hour light streaming through a window, highlighting the fine texture of the clay and the fabric of his apron. Captured with an 85mm portrait lens, resulting in a soft, blurred background (bokeh). The overall mood is serene and masterful.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('photorealistic_example.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzSfgs7bgAAYkJ_?format=jpg&name=900x900)
A photorealistic close-up portrait of an elderly Japanese ceramicist...

---

## 2. Stylized Illustrations & Stickers

To create stickers, icons, or assets for your projects, be explicit about the style and remember to request a white background if you need one.

> Template: A [style] sticker of a [subject], featuring [key characteristics] and a [color palette]. The design should have [line style] and [shading style]. The background must be white.

> Example Prompt: A kawaii-style sticker of a happy red panda wearing a tiny bamboo hat. It's munching on a green bamboo leaf. The design features bold, clean outlines, simple cel-shading, and a vibrant color palette. The background must be white.

```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="A kawaii-style sticker of a happy red panda wearing a tiny bamboo hat. It's munching on a green bamboo leaf. The design features bold, clean outlines, simple cel-shading, and a vibrant color palette. The background must be white.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('red_panda_sticker.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzSgnA0asAENRdS?format=jpg&name=900x900)
A kawaii-style sticker of a happy red panda...

---

## 3. Accurate Text in Images

Gemini excels at rendering text. Be clear about the text, the font style (descriptively), and the overall design.

> Template: Create a [image type] for [brand/concept] with the text "[text to render]" in a [font style]. The design should be [style description], with a [color scheme].

> Prompt: Create a modern, minimalist logo for a coffee shop called 'The Daily Grind'. The text should be in a clean, bold, sans-serif font. The design should feature a simple, stylized icon of a coffee bean seamlessly integrated with the text. The color scheme is black and white.

```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="Create a modern, minimalist logo for a coffee shop called 'The Daily Grind'. The text should be in a clean, bold, sans-serif font. The design should feature a simple, stylized icon of a a coffee bean seamlessly integrated with the text. The color scheme is black and white.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('logo_example.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzShMC6b0AEgNr4?format=jpg&name=900x900)
Create a modern, minimalist logo for a coffee shop called 'The Daily Grind'...

---

## 4. Product Mockups & Commercial Photography

Create clean, professional product shots for e-commerce, advertising, or branding.

> Template: A high-resolution, studio-lit product photograph of a [product description] on a [background surface/description]. The lighting is a [lighting setup, e.g., three-point softbox setup] to [lighting purpose]. The camera angle is a [angle type] to showcase [specific feature]. Ultra-realistic, with sharp focus on [key detail]. [Aspect ratio].

> Example Prompt: A high-resolution, studio-lit product photograph of a minimalist ceramic coffee mug in matte black, presented on a polished concrete surface. The lighting is a three-point softbox setup designed to create soft, diffused highlights and eliminate harsh shadows. The camera angle is a slightly elevated 45-degree shot to showcase its clean lines. Ultra-realistic, with sharp focus on the steam rising from the coffee. Square image.

```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="A high-resolution, studio-lit product photograph of a minimalist ceramic coffee mug in matte black, presented on a polished concrete surface. The lighting is a three-point softbox setup designed to create soft, diffused highlights and eliminate harsh shadows. The camera angle is a slightly elevated 45-degree shot to showcase its clean lines. Ultra-realistic, with sharp focus on the steam rising from the coffee. Square image.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('product_mockup.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzShcOsboAAo2bM?format=jpg&name=900x900)
A high-resolution, studio-lit product photograph of a minimalist ceramic coffee mug...

---

## 5. Minimalist & Negative Space Design

Excellent for creating backgrounds for websites, presentations, or marketing materials where text will be overlaid.

> Template: A minimalist composition featuring a single [subject] positioned in the [bottom-right/top-left/etc.] of the frame. The background is a vast, empty [color] canvas, creating significant negative space. Soft, subtle lighting. [Aspect ratio].

> Example Prompt: A minimalist composition featuring a single, delicate red maple leaf positioned in the bottom-right of the frame. The background is a vast, empty off-white canvas, creating significant negative space for text. Soft, diffused lighting from the top left. Square image.

```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="A minimalist composition featuring a single, delicate red maple leaf positioned in the bottom-right of the frame. The background is a vast, empty off-white canvas, creating significant negative space for text. Soft, diffused lighting from the top left. Square image.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('minimalist_design.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzShwQyakAALmHV?format=jpg&name=900x900)
A minimalist composition featuring a single, delicate red maple leaf...

---

## 6. Sequential Art (Comic Panel / Storyboard)

Create compelling visual narratives, panel by panel, ideal for developing storyboards, comic strips, or any form of sequential art by focusing on clear scene descriptions.

> Template: A single comic book panel in a [art style] style. In the foreground, [character description and action]. In the background, [setting details]. The panel has a [dialogue/caption box] with the text "[Text]". The lighting creates a [mood] mood. [Aspect ratio].

> Example Prompt: A single comic book panel in a gritty, noir art style with high-contrast black and white inks. In the foreground, a detective in a trench coat stands under a flickering streetlamp, rain soaking his shoulders. In the background, the neon sign of a desolate bar reflects in a puddle. A caption box at the top reads "The city was a tough place to keep secrets." The lighting is harsh, creating a dramatic, somber mood. Landscape.

```
from google import genai
from google.genai import types
from PIL import Image
from io import BytesIO

client = genai.Client()

# Generate an image from a text prompt
response = client.models.generate_content(
    model="gemini-2.5-flash-image-preview",
    contents="A single comic book panel in a gritty, noir art style with high-contrast black and white inks. In the foreground, a detective in a trench coat stands under a flickering streetlamp, rain soaking his shoulders. In the background, the neon sign of a desolate bar reflects in a puddle. A caption box at the top reads \"The city was a tough place to keep secrets.\" The lighting is harsh, creating a dramatic, somber mood. Landscape.",
)

image_parts = [
    part.inline_data.data
    for part in response.candidates[0].content.parts
    if part.inline_data
]

if image_parts:
    image = Image.open(BytesIO(image_parts[0]))
    image.save('comic_panel.png')
    image.show()
```

![图像](https://pbs.twimg.com/media/GzSjX3lasAEAI98?format=jpg&name=900x900)
A single comic book panel in a gritty, noir art style...





