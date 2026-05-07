# Claude Design Tutorial 

**Created**: 2026-05-05 16:10
**Source**: `x.com/viktoroddy/status/2045492112054165813`
**Primary Tool**: `claude.ai/design`
**Animated Website Resource**: `motionsites.ai/`
**Imager Generation Resource**: `higgsfield.ai/image/nano-banana-2`

---

## Resources 

Explore what comes with my PRO subscription: `aistudio.google.com`
Inspiration: `land-book.com`
Template websites to copy into prompt `motionsites.ai`
Animated looped background: `motionsites.ai/backgrounds`

## Flow 

  1. Go to Claude Design > Select "Other" > Name the project.
  2. Find an example template at `motionsites.ai`
     - Select based on the *TYPOGRAPHY* and *UI*
     - Expect that you will be able to change the visual imagery/video
     - You will be able to remake any of the UI elements as well based on needs 
  3. Copy site & paste into Claude Design with prompt
     > Build this out but do not add any of the video elements, I'm going to replace that later.
     - Claude builds the website as expected
     - Next objective is to get a background video to replace the original 
  4. Get new background video 
     - Using Nano Banana 2 `higgsfield.ai/image/nano-banana-2` prompt
     > A single amorphous translucent blob floating in deep black space, organic shape resembling a soft liquid droplet with smooth rounded contours, slightly asymmetrical like a stretched bubble. The surface is glass-like and semi-transparent with strong subsurface scattering. Edges glow with bright white rim lighting, creating a halo effect. The interior shows subtle iridescent rainbow refractions (blue, gold, orange, and hints of violet), similar to oil-on-water interference. The center remains darker and slightly opaque.
     > Lighting is cinematic and minimal: one strong soft light source from the lett creating a bright highlight, with subtle reflections across the surface. Background is pure black with faint tiny star-like specks scattered sparsely.
     > Texture is ultra-smooth, glossy, liquid-glass material with high reflectivity and soft gradients. The blob appears weightless and suspended.
     > Composition is centered horizontally and slightly lower than the vertical center, occupying roughly 40-50% of the frame width.
     > Depth of field is shallow with soft focus edges and slight bloom/glow on highlights.
     > Style is hyper-realistic 3D render, abstract, cinematic, high dynamic range, Octane/Redshift render style, ultra clean, minimal aesthetic.
     - Find video background already created via `motionsites.ai/backgrounds`
  5. Create animation using still image
     - Uses Seed Dance 2 `higgsfield.ai/create/video`
     > "Create video animation like this but without any text of ui element. I just want the animated background looping animation. No camera movement, no extra element to be added, no zoom in and no zoom out, looping animation."
     - 1080p 7.0s 16:9
  6. Have Claude Design Replace Animation
     - Copies the .mp4 URL of the video and says
       > "Replace the video background to be this one"
  7. Find UI Elements
     - Search on Dribbble
     - Wherever they're from, just need to find screenshots of what is wanted to show Claude Design
     - Zoomed into random website he liked a random block with unrelated text and then copied the UI element, screenshot of two elements.
  8. Ask Claude Design to add new UI
     > "Add these two UI elements under the text in our hero section, positioned nicely under our content and they should not be this color, they should be the same liquid glass that we used already for the nav bar and for the buttons. The text should be white."
  9. Preview Mobile Version
     - Came back with really pretty cards
     - Before moving forward, check mobile version
     - If good, go to build rest of site or next section, etc. 
  10. Start next section with random image
      - He found image he liked on Pinterest, removed text, elements, etc. 
      - Went to Nano Banana 2 and pasted in a cropped screenshot with prompt
      > Create me an image like this in 8k, remove the text, buttons, any logos, just want exact same background. Same positioning, no zoom in or zoom out. Plain black background.
      - Actually used the 4K setting though
  11. Make new image match style
      - Reference resulting image that was just created
      - Reference the original image used to create the first animation
      - Prompt something along the lines of
      > Make the second (attached) image in the same style as the first image; the background should be the same, the texture should be the same, and it should otherwise look similar while maintaining the current second image composition.
      > Create for me the second image but in the same style as the first image.
      > Make the second image have the same background and color as the first image. Basically, copy the styles of the first image and apply them to the second image without losing too much of the second image because I like that.
  12. Animate created image
      - Upload new image to tool, he is using "Kling 3.0" with prompt
      > Looping animation. No camera movement, no extra elements to add, no zoom in or out.
      - 1080p 12.0s 16:9
      - He generative expanded the image in one case (experimenting to see what works best), then used super simple prompt
      > Animate this
  13. Finish putting together the section
      - He found/used the image that the background was pulled from and made it simpler for the AI with Nano Banana 3 prompt
      > Create me an image like this in 8k, same layout of text. Remove the background, just keep the text and any cards and icons on plain black background. I don't want the background image.
      - The result looks like a wireframe which is what was needed as a sample image to provide with the video URL
      > Build out this section as it is, under the hero section. Use this video for the background URL
