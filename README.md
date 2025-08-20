# The Complete Flux AI Image Generation Guide

*Master the art of AI image creation with camera techniques, lighting setups, and artistic styles*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

## Table of Contents

- [Getting Started](#getting-started)
- [Flux Models & Configuration](#flux-models--configuration)
- [Seeds & Reproducibility](#seeds--reproducibility)
- [Start Images & Img2Img](#start-images--img2img)
- [Camera Techniques](#camera-techniques)
- [Lighting Mastery](#lighting-mastery)
- [Art Styles & Movements](#art-styles--movements)
- [Advanced Prompting](#advanced-prompting)
- [Quality Modifiers](#quality-modifiers)
- [Common Mistakes](#common-mistakes)
- [Example Prompts](#example-prompts)
- [Contributing](#contributing)

## Getting Started

Flux AI excels at understanding detailed photographic and artistic terminology. The key to stunning results is combining specific technical language with creative vision. This guide provides the vocabulary and techniques to unlock Flux's full potential.

### Basic Prompt Structure
```
[Subject] + [Camera/Lens] + [Lighting] + [Art Style] + [Quality Modifiers]
```

**Example:**
```
Portrait of a woman, shot with 85mm lens, golden hour lighting, in the style of Annie Leibovitz, highly detailed, professional photography
```

## Flux Models & Configuration

### Available Flux Models

#### 🚀 **Flux.1 [pro]** - Premium Model
- **Best for**: Highest quality, commercial work, complex scenes
- **Strengths**: Superior detail, better prompt adherence, advanced lighting
- **Use case**: Professional photography, advertising, detailed artwork
- **Access**: API only, paid tier

#### ⚡ **Flux.1 [dev]** - Development Model  
- **Best for**: High-quality generation, creative projects
- **Strengths**: Excellent balance of quality and speed, great for experimentation
- **Use case**: Most general purposes, artistic creation, prototyping
- **Access**: HuggingFace, RunPod, local installation

#### 🔓 **Flux.1 [schnell]** - Fast Model
- **Best for**: Rapid iteration, testing prompts, batch generation
- **Strengths**: Very fast generation (2-4 steps), good quality for speed
- **Use case**: Quick concepts, prompt testing, high-volume generation
- **Access**: Free tier on many platforms

### Model Selection Guidelines

| Use Case | Recommended Model | Why |
|----------|------------------|-----|
| Professional photography | Flux.1 [pro] | Maximum detail and realism |
| Artistic creation | Flux.1 [dev] | Best quality/time balance |
| Concept exploration | Flux.1 [schnell] | Fast iteration cycles |
| Batch processing | Flux.1 [schnell] | Speed and efficiency |
| Commercial work | Flux.1 [pro] | Highest quality output |

### Generation Parameters

#### 🎛️ **Key Settings**
```python
# Typical parameter ranges
steps: 20-50          # More steps = higher quality (diminishing returns after 30)
guidance_scale: 3.5-7.5   # Higher = more prompt adherence (7.5 is standard)
width: 512-1536       # Image width in pixels
height: 512-1536      # Image height in pixels
seed: 0-4294967295    # For reproducible results
```

#### ⚙️ **Advanced Parameters**
```python
# Fine-tuning options
scheduler: "euler_a"  # Sampling method (euler_a, dpm++, etc.)
num_inference_steps: 28  # Quality vs speed trade-off
strength: 0.7         # For img2img - how much to change (0.1-1.0)
controlnet_conditioning_scale: 1.0  # ControlNet influence
```

## Seeds & Reproducibility

### Understanding Seeds

A **seed** is a number that controls the randomness in AI generation. Same seed + same prompt + same settings = identical image.

#### 🎲 **Seed Basics**
- **Range**: 0 to 4,294,967,295 (32-bit integer)
- **Default**: Random seed if not specified
- **Purpose**: Reproduce exact results or create variations

#### 🔄 **Seed Strategies**

**For Exploration:**
```python
# Let system choose random seeds
seed: -1  # or omit seed parameter
# Good for: Initial exploration, finding good compositions
```

**For Refinement:**
```python
# Use specific seed from good result
seed: 42891057
# Good for: Tweaking prompts while keeping composition
```

**For Variations:**
```python
# Increment seed for similar but different results
seed: 42891057  # Original
seed: 42891058  # Slight variation
seed: 42891059  # Another variation
```

### Practical Seed Workflows

#### 🎯 **The Seed Discovery Process**

1. **Initial Generation** (Random Seeds)
   ```
   Prompt: "Portrait of a woman, golden hour lighting"
   Seed: Random
   Generate: 4-8 images
   ```

2. **Identify Best Composition**
   ```
   Best result seed: 1847362819
   Save this seed for refinement
   ```

3. **Prompt Refinement** (Fixed Seed)
   ```
   Seed: 1847362819 (kept constant)
   Prompt iterations:
   - "Portrait of a woman, golden hour lighting, 85mm lens"
   - "Portrait of a woman, golden hour lighting, 85mm lens, film photography"
   - "Portrait of a woman, golden hour lighting, 85mm lens, Kodak Portra 400"
   ```

4. **Final Variations** (Seed Range)
   ```
   Final prompt + seeds: 1847362819, 1847362820, 1847362821
   Choose best final result
   ```

#### 📊 **Seed Organization**
```
Project: Fashion Shoot
├── Exploration Seeds: Random (discovery phase)
├── Golden Seeds: 1847362819, 2956184730 (keeper compositions)
├── Variation Seeds: +1, +2, +3 from golden seeds
└── Final Seeds: Selected seeds for delivery
```

## Start Images & Img2Img

### Image-to-Image Workflow

Img2Img uses an existing image as a starting point, allowing you to transform photos into different styles while maintaining composition.

#### 🖼️ **Start Image Preparation**

**Optimal Start Images:**
- **Resolution**: Match your target output (1024x1024, 1536x1024, etc.)
- **Composition**: Strong, clear composition translates better
- **Lighting**: Good contrast and defined shadows/highlights
- **Subject**: Clear subject definition for better transformation

**Image Preprocessing:**
```python
# Resize to target dimensions
target_size: (1024, 1024)
resize_method: "crop_center"  # or "stretch", "pad"

# Optional: Enhance contrast/clarity before input
preprocessing: "auto_contrast"
```

#### ⚙️ **Strength Parameter Control**

The `strength` parameter controls how much the AI changes your start image:

| Strength | Effect | Use Case |
|----------|--------|----------|
| 0.1-0.3 | Minimal changes | Color correction, lighting adjustment |
| 0.4-0.6 | Moderate transformation | Style transfer, artistic interpretation |
| 0.7-0.8 | Major changes | Complete style overhaul |
| 0.9-1.0 | Nearly from scratch | Use composition only |

#### 🎨 **Img2Img Techniques**

**Style Transfer:**
```python
start_image: "portrait_photo.jpg"
prompt: "Oil painting style, thick brushstrokes, Renaissance lighting"
strength: 0.6
seed: 1234567
```

**Photo Enhancement:**
```python
start_image: "smartphone_photo.jpg"  
prompt: "Professional photography, studio lighting, high detail, DSLR quality"
strength: 0.4
seed: 1234567
```

**Artistic Interpretation:**
```python
start_image: "landscape_photo.jpg"
prompt: "Van Gogh style, swirling brushstrokes, vivid colors, post-impressionist"
strength: 0.7
seed: 1234567
```

**Composition Preservation:**
```python
start_image: "rough_sketch.jpg"
prompt: "Photorealistic portrait, professional lighting, detailed features"
strength: 0.8
seed: 1234567
```

### Advanced Img2Img Workflows

#### 🔄 **Multi-Stage Refinement**

**Stage 1: Base Transformation**
```python
input: original_photo.jpg
prompt: "Digital painting style, fantasy art"
strength: 0.7
output: stage1_result.jpg
```

**Stage 2: Detail Enhancement**
```python
input: stage1_result.jpg
prompt: "Highly detailed, sharp focus, professional digital art"
strength: 0.3
output: stage2_result.jpg
```

**Stage 3: Final Polish**
```python
input: stage2_result.jpg
prompt: "Award-winning artwork, perfect lighting, masterpiece quality"
strength: 0.2
output: final_result.jpg
```

#### 🎭 **Style Exploration Workflow**

**Base Setup:**
```python
start_image: "portrait_base.jpg"
seed: 987654321  # Keep constant for comparison
strength: 0.6    # Moderate transformation
```

**Style Variations:**
```python
# Photographic styles
prompt: "Film noir lighting, black and white, dramatic shadows"
prompt: "Golden hour portrait, warm lighting, film photography"
prompt: "Studio portrait, professional lighting, fashion photography"

# Artistic styles  
prompt: "Oil painting, Renaissance style, classical lighting"
prompt: "Watercolor painting, soft edges, impressionist style"
prompt: "Digital art, concept art style, fantasy lighting"
```

#### 🖌️ **Sketch to Finished Art**

**From Rough Sketch:**
```python
start_image: "rough_pencil_sketch.jpg"
prompt: "Detailed character design, professional concept art, full color"
strength: 0.8
guidance_scale: 7.5
```

**From Photo Reference:**
```python
start_image: "reference_photo.jpg"
prompt: "Anime character design, cel shading, studio lighting"
strength: 0.7
guidance_scale: 8.0
```

### ControlNet Integration

#### 🎮 **ControlNet Types**

**Canny Edge Detection:**
```python
controlnet: "canny"
start_image: "photo.jpg"
prompt: "Line art, clean linework, black and white illustration"
controlnet_conditioning_scale: 1.0
```

**Depth Map:**
```python
controlnet: "depth"
start_image: "photo.jpg"  
prompt: "3D render, volumetric lighting, octane render"
controlnet_conditioning_scale: 0.8
```

**Pose Detection:**
```python
controlnet: "openpose"
start_image: "person_photo.jpg"
prompt: "Fantasy character, detailed armor, epic lighting"
controlnet_conditioning_scale: 1.0
```

### Batch Processing with Seeds

#### 🔄 **Systematic Exploration**

```python
# Base configuration
start_image: "base_photo.jpg"
prompt: "Professional portrait, studio lighting"
strength: 0.6

# Seed batch for variations
seeds: [12345, 12346, 12347, 12348, 12349]
# Generates 5 variations with same prompt and start image

# Alternative: Prompt variations with fixed seed
seed: 12345
prompts: [
    "Professional portrait, studio lighting, warm tones",
    "Professional portrait, studio lighting, cool tones", 
    "Professional portrait, studio lighting, dramatic contrast",
    "Professional portrait, studio lighting, soft and dreamy"
]
```

## Camera Techniques

### Camera Types & Characteristics

#### 📷 **35mm Film Cameras**
- `35mm film photography` - Classic grain and color rendition
- `Leica aesthetic` - Sharp, contrasty, premium look
- `Kodak Portra 400` - Warm, natural skin tones
- `Fuji film stock` - Vibrant colors, distinctive character
- `black and white film` - Timeless, dramatic contrast

#### 📸 **Digital Camera Systems**
- `shot on Canon 5D Mark IV` - Professional DSLR quality
- `Sony A7R IV photography` - High resolution, excellent detail
- `Fujifilm X-T4 aesthetic` - Film-inspired color science
- `medium format camera` - Exceptional detail and dynamic range
- `large format photography` - Ultimate sharpness and depth

#### 📱 **Specialized Cameras**
- `Polaroid instant photo` - Vintage, square format, faded colors
- `disposable camera aesthetic` - Lo-fi, overexposed, casual
- `film camera with flash` - Harsh shadows, authentic snapshot feel
- `drone photography` - Aerial perspective, vast landscapes
- `security camera footage` - Grainy, surveillance aesthetic

### Lens Types & Effects

#### 🔍 **Wide Angle Lenses**
- `14mm ultra-wide lens` - Dramatic perspective, environmental context
- `24mm wide angle` - Expansive view, slight distortion
- `35mm lens` - Natural perspective, street photography feel
- `fisheye lens` - Extreme distortion, creative effect
- `architectural photography lens` - Corrected perspective lines

#### 🎯 **Standard & Portrait Lenses**
- `50mm lens` - Natural human vision perspective
- `85mm portrait lens` - Beautiful bokeh, subject isolation
- `135mm telephoto` - Compressed perspective, intimate feel
- `macro lens photography` - Extreme close-up detail
- `tilt-shift lens` - Selective focus, miniature effect

#### 🔭 **Telephoto & Specialty**
- `200mm telephoto` - Compressed background, wildlife feel
- `400mm super telephoto` - Sports photography aesthetic
- `vintage lens character` - Soft rendering, character flaws
- `anamorphic lens` - Cinematic bokeh, horizontal flares
- `infrared photography` - Surreal, otherworldly colors

### Camera Settings & Techniques

#### ⚙️ **Exposure Settings**
- `shot at f/1.4` - Extremely shallow depth of field
- `f/2.8 aperture` - Good subject separation
- `f/8 aperture` - Sharp throughout, landscape ideal
- `f/16 small aperture` - Everything in focus, sunstar effects
- `long exposure` - Motion blur, light trails
- `high shutter speed` - Frozen action, crisp movement

#### 🎨 **Depth of Field**
- `shallow depth of field` - Blurred background, subject focus
- `bokeh photography` - Beautiful out-of-focus areas
- `deep focus` - Everything sharp, environmental storytelling
- `rack focus` - Shifting focus between subjects
- `hyperfocal distance` - Maximum sharpness throughout scene

#### 📐 **Composition Techniques**
- `rule of thirds composition` - Balanced, pleasing arrangement
- `leading lines` - Guide viewer's eye through image
- `symmetrical composition` - Formal, architectural feel
- `golden ratio composition` - Naturally pleasing proportions
- `negative space` - Minimalist, breathing room
- `dutch angle` - Tilted, dynamic tension

## Lighting Mastery

### Natural Lighting

#### ☀️ **Time of Day**
- `golden hour lighting` - Warm, soft, magical quality
- `blue hour photography` - Deep blue sky, city lights
- `harsh midday sun` - Strong shadows, high contrast
- `overcast lighting` - Soft, even, natural diffusion
- `sunrise lighting` - Fresh, hopeful, warm tones
- `sunset glow` - Romantic, dramatic, orange hues

#### 🌤️ **Weather & Atmosphere**
- `dramatic storm lighting` - Dark clouds, shaft of light
- `fog photography` - Mysterious, ethereal atmosphere
- `backlighting` - Rim light, silhouettes, dramatic contrast
- `diffused sunlight` - Soft, flattering, even illumination
- `dappled light` - Filtered through leaves, spotted patterns
- `window light portrait` - Natural, soft, directional

### Studio Lighting

#### 💡 **Professional Setups**
- `studio lighting setup` - Controlled, professional quality
- `key light and fill light` - Classic portrait lighting ratio
- `Rembrandt lighting` - Triangular cheek highlight
- `butterfly lighting` - Symmetrical nose shadow
- `split lighting` - Half face in shadow, dramatic
- `rim lighting` - Edge light separation from background

#### 🎭 **Dramatic Lighting**
- `chiaroscuro lighting` - Strong light/dark contrast
- `film noir lighting` - Venetian blind shadows, mystery
- `single light source` - Dramatic, moody atmosphere
- `candlelight` - Warm, flickering, intimate
- `neon lighting` - Colorful, urban, contemporary
- `practical lighting` - Lights visible in scene

#### 🌈 **Color Temperature**
- `warm tungsten lighting` - Orange/yellow, cozy feel
- `cool daylight` - Blue tone, crisp and clean
- `mixed color temperature` - Creative color contrast
- `LED panel lighting` - Even, controllable, modern
- `fluorescent lighting` - Green tint, office aesthetic
- `HMI lighting` - Daylight balanced, professional film

### Creative Lighting Effects

#### ✨ **Special Effects**
- `volumetric lighting` - Visible light beams, atmospheric
- `lens flare` - Sun hitting lens, authentic feel
- `light painting` - Long exposure light trails
- `strobe lighting` - Frozen motion, party atmosphere
- `colored gels` - Creative color effects
- `gobo patterns` - Projected shadows and shapes

#### 🔥 **Mood Lighting**
- `moody lighting` - Low key, atmospheric
- `high key lighting` - Bright, even, optimistic
- `dramatic side lighting` - Strong directional shadows
- `uplighting` - Theatrical, unnatural direction
- `backlighting silhouette` - Subject as dark shape
- `edge lighting` - Outline subject with light

## Art Styles & Movements

### Photography Styles

#### 📸 **Classic Photography**
- `Ansel Adams style` - Dramatic landscapes, zone system
- `Henri Cartier-Bresson` - Decisive moment, street photography
- `Annie Leibovitz portrait` - Celebrity portraiture, dramatic lighting
- `Richard Avedon fashion` - High contrast, dynamic poses
- `Vivian Maier aesthetic` - Street photography, human moments
- `Steve McCurry style` - Travel photography, vibrant colors

#### 🎨 **Contemporary Photography**
- `Gregory Crewdson style` - Cinematic, suburban surrealism
- `Andreas Gursky` - Large format, architectural precision
- `Cindy Sherman aesthetic` - Conceptual self-portraits
- `David LaChapelle` - Hyperreal, pop surrealism
- `Tim Walker photography` - Whimsical, fashion fantasy
- `Peter Lindbergh style` - Black and white, natural beauty

### Digital Art Styles

#### 🖥️ **Digital Techniques**
- `digital painting` - Painterly, artistic interpretation
- `photorealistic rendering` - 3D quality, perfect detail
- `concept art style` - Gaming/film development aesthetic
- `matte painting` - Digital environment creation
- `HDR photography` - High dynamic range, vivid colors
- `focus stacking` - Perfect sharpness throughout

#### 🎮 **Modern Aesthetics**
- `cyberpunk aesthetic` - Neon, dark, futuristic
- `vaporwave style` - Retro-futuristic, pink/purple
- `synthwave art` - 80s nostalgia, neon grids
- `minimalist photography` - Clean, simple, negative space
- `maximalist composition` - Busy, detailed, overwhelming
- `glitch art aesthetic` - Digital errors, corruption

### Classic Art Movements

#### 🎨 **Traditional Techniques**
- `oil painting style` - Rich colors, visible brushstrokes
- `watercolor painting` - Transparent, flowing, delicate
- `charcoal drawing` - Dramatic blacks, soft grays
- `pencil sketch` - Detailed linework, realistic rendering
- `ink wash painting` - Asian aesthetic, flowing brushwork
- `pastel drawing` - Soft colors, impressionistic

#### 🏛️ **Art Historical Periods**
- `Renaissance painting` - Classical composition, religious themes
- `Baroque style` - Dramatic lighting, emotional intensity
- `Impressionist painting` - Loose brushwork, light studies
- `Art Nouveau` - Organic forms, decorative elements
- `Art Deco style` - Geometric patterns, luxury aesthetic
- `Pop art style` - Bold colors, commercial imagery

#### 🌟 **Iconic Artists**
- `Van Gogh style` - Swirling brushstrokes, emotional color
- `Picasso cubist` - Geometric forms, multiple perspectives
- `Monet impressionist` - Light studies, atmospheric effects
- `Dali surrealism` - Dreamlike, impossible imagery
- `Warhol pop art` - Repetition, commercial techniques
- `Basquiat style` - Raw expression, text elements

## Advanced Prompting

### Combining Elements

#### 🎯 **Layered Descriptions**
```
Portrait of a jazz musician, shot with vintage Leica M6, 
Kodak Tri-X film, available light from club window, 
in the style of William Claxton, high contrast black and white, 
film grain visible, authentic 1960s aesthetic
```

#### 🔄 **Style Mixing**
```
Cyberpunk street scene, shot on medium format film camera,
neon lighting mixed with natural fog, 
Blade Runner aesthetic meets Vivian Maier composition,
high detail, cinematic color grading
```

### Technical Specifications

#### 📏 **Aspect Ratios**
- `16:9 cinematic` - Widescreen, film aesthetic
- `4:3 classic` - Traditional photography format
- `1:1 square` - Instagram, symmetrical composition
- `9:16 vertical` - Mobile, portrait orientation
- `21:9 ultrawide` - Panoramic, epic landscapes
- `2.35:1 anamorphic` - Cinema scope, dramatic

#### 🎚️ **Quality Controls**
- `8K resolution` - Maximum detail and sharpness
- `RAW file quality` - Unprocessed, maximum data
- `professional color grading` - Polished, cinematic look
- `film grain texture` - Authentic analog character
- `perfect exposure` - Technically excellent capture
- `tack sharp focus` - Crystal clear subject detail

### Emotional & Atmospheric

#### 💭 **Mood Descriptors**
- `melancholic atmosphere` - Sad, contemplative feeling
- `euphoric energy` - Joyful, uplifting emotion
- `tense anticipation` - Suspenseful, dramatic moment
- `serene tranquility` - Peaceful, calming presence
- `nostalgic warmth` - Memory-evoking, sentimental
- `mysterious intrigue` - Enigmatic, thought-provoking

#### 🌍 **Environmental Context**
- `urban decay` - Abandoned, post-industrial aesthetic
- `pristine nature` - Untouched, natural beauty
- `cozy interior` - Warm, inviting indoor space
- `vast landscape` - Epic scale, environmental grandeur
- `intimate setting` - Close, personal, private moment
- `bustling crowd` - Energy, movement, human activity

## Quality Modifiers

### Enhancement Keywords

#### ⭐ **Universal Quality**
- `highly detailed` - Maximum texture and definition
- `photorealistic` - Believable, real-world quality
- `professional photography` - Commercial, polished result
- `award-winning` - Exhibition quality, exceptional
- `masterpiece` - Artistic excellence, museum quality
- `trending on ArtStation` - Contemporary digital art quality

#### 🔍 **Technical Excellence**
- `tack sharp` - Perfect focus, crisp detail
- `perfect composition` - Ideal arrangement of elements
- `studio quality` - Professional lighting and setup
- `color accurate` - True-to-life color reproduction
- `dynamic range` - Full spectrum of tones
- `no digital artifacts` - Clean, artifact-free rendering

### Style Intensifiers

#### 🎨 **Artistic Enhancement**
- `in the style of [artist name]` - Specific artistic influence
- `reminiscent of [photographer]` - Photographic style reference
- `[art movement] inspired` - Historical art reference
- `contemporary [style]` - Modern interpretation
- `vintage [era] aesthetic` - Period-specific look
- `fine art photography` - Gallery-worthy artistic quality

## Common Mistakes

### ❌ **What to Avoid**

#### Overly Complex Prompts
```
BAD: "Ultra realistic 8K HDR professional award-winning masterpiece 
highly detailed photorealistic perfect composition studio lighting 
portrait of a woman with perfect skin and beautiful eyes..."
```

```
GOOD: "Portrait of a woman, shot with 85mm lens, 
natural window light, in the style of Annie Leibovitz"
```

#### Contradictory Instructions
```
BAD: "Shallow depth of field with everything in sharp focus"
GOOD: "Shallow depth of field, subject in focus, background bokeh"
```

#### Vague Descriptions
```
BAD: "Nice lighting, good quality, beautiful"
GOOD: "Golden hour backlighting, film photography, warm tones"
```

### ✅ **Best Practices**

1. **Be Specific**: Use precise technical terms
2. **Layer Information**: Build complexity gradually
3. **Reference Masters**: Name specific artists or photographers
4. **Balance Elements**: Don't overload with modifiers
5. **Test Variations**: Iterate and refine your prompts

## Example Prompts

### Portrait Photography

#### Classic Portrait
```
Professional headshot of a business executive, shot with 85mm lens, 
soft studio lighting with key and fill lights, 
in the style of Peter Hurley, clean background, 
sharp focus, corporate photography
```

#### Environmental Portrait
```
Portrait of an elderly craftsman in his workshop, 
natural window light, shot with 35mm lens, 
shallow depth of field, authentic documentary style, 
in the style of Steve McCurry, warm tones, storytelling
```

#### Fashion Portrait
```
High fashion portrait, shot with medium format camera, 
dramatic studio lighting, Rembrandt lighting setup, 
in the style of Richard Avedon, high contrast black and white, 
elegant pose, luxury aesthetic
```

### Landscape Photography

#### Golden Hour Landscape
```
Mountain landscape at sunrise, shot with wide-angle lens, 
golden hour lighting, dramatic clouds, 
in the style of Ansel Adams, high dynamic range, 
black and white, fine art photography
```

#### Urban Landscape
```
Cyberpunk cityscape at night, neon lighting, 
rain-soaked streets reflecting lights, 
shot with 24mm lens, long exposure for light trails, 
Blade Runner aesthetic, cinematic color grading
```

### Street Photography

#### Candid Moment
```
Street photography, candid moment of people crossing busy intersection, 
shot with 35mm lens, available light, 
in the style of Henri Cartier-Bresson, decisive moment, 
black and white, human geometry
```

#### Urban Life
```
NYC street scene, golden hour side lighting, 
shot with vintage film camera, Kodak Portra 400, 
in the style of Vivian Maier, authentic grain, 
warm color palette, slice of life
```

### Artistic & Creative

#### Surreal Art
```
Surreal portrait with floating elements, 
dramatic studio lighting, impossible architecture, 
in the style of René Magritte meets Gregory Crewdson, 
hyperrealistic rendering, dreamlike atmosphere
```

#### Minimalist Composition
```
Minimalist architectural photography, 
clean geometric lines, negative space, 
shot with tilt-shift lens, even lighting, 
in the style of Andreas Gursky, contemporary art
```

## Contributing

We welcome contributions to improve this guide! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-technique`)
3. Add your improvements with examples
4. Test prompts with Flux AI
5. Submit a pull request

### Areas for Contribution

- New camera techniques and lenses
- Additional lighting setups
- Emerging art styles and trends
- Cultural and regional photography styles
- Specialized photography genres (macro, astrophotography, etc.)
- More example prompts with results

## License

This guide is released under the MIT License. Feel free to use, modify, and distribute.

---

*Happy creating with Flux AI! Remember: the best prompt is the one that captures your creative vision.*

**Found this helpful? ⭐ Star this repository and share it with other AI artists!**
