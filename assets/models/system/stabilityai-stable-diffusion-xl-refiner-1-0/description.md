[SDXL](https://arxiv.org/abs/2307.01952) consists of an [ensemble of experts](https://arxiv.org/abs/2211.01324) pipeline for latent diffusion: 
In a first step, the base model (available here: https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0) is used to generate (noisy) latents, which are then further processed with a refinement model specialized for the final denoising steps. Note that the base model can be used as a standalone module.

Alternatively, we can use a two-stage pipeline as follows:
First, the base model is used to generate latents of the desired output size. In the second step, we use a specialized high-resolution model and apply a technique called SDEdit (https://arxiv.org/abs/2108.01073, also known as "img2img") to the latents generated in the first step, using the same prompt. This technique is slightly slower than the first one, as it requires more function evaluations.

The model is intended for research purposes only. Possible research areas and tasks include

- Generation of artworks and use in design and other artistic processes.
- Applications in educational or creative tools.
- Research on generative models.
- Safe deployment of models which have the potential to generate harmful content.
- Probing and understanding the limitations and biases of generative models.

# Evaluation Results

![comparison](https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-1.0/blob/main/comparison.png)
The chart above evaluates user preference for SDXL (with and without refinement) over SDXL 0.9 and Stable Diffusion 1.5 and 2.1. The SDXL base model performs significantly better than the previous variants, and the model combined with the refinement module achieves the best overall performance.

# Limitations and Biases

## Limitations

- The model does not achieve perfect photorealism
- The model cannot render legible text
- The model struggles with more difficult tasks which involve compositionality, such as rendering an image corresponding to “A red cube on top of a blue sphere”
- Faces and people in general may not be generated properly.
- The autoencoding part of the model is lossy.

## Bias

While the capabilities of image generation models are impressive, they can also reinforce or exacerbate social biases.

## Out-of-Scope Use

The model was not trained to be factual or true representations of people or events, and therefore using the model to generate such content is out-of-scope for the abilities of this model.

# Inference Samples

Inference type|Python sample (Notebook)|CLI with YAML
|--|--|--|
Real time|<a href="https://aka.ms/azureml-infer-sdk-image-text-to-image-generation" target="_blank">image-text-to-image-online-endpoint.ipynb</a>|<a href="https://aka.ms/azureml-infer-cli-image-text-to-image-generation" target="_blank">image-text-to-image-online-endpoint.sh</a>
Batch |<a href="https://aka.ms/azureml-infer-batch-sdk-image-text-to-image-generation" target="_blank">image-text-to-image-batch-endpoint.ipynb</a>|<a href="https://aka.ms/azureml-infer-batch-cli-image-text-to-image-generation" target="_blank">image-text-to-image-batch-endpoint.sh</a>

<h3> Inference with <a href="https://learn.microsoft.com/en-us/azure/ai-services/content-safety/studio-quickstart", target="_blank">Azure AI Content Safety (AACS)</a> samples </h3>

Inference type|Python sample (Notebook)
|--|--|
Real time|<a href="https://aka.ms/azureml-infer-sdk-safe-image-text-to-image-generation" target="_blank">safe-image-text-to-image-online-endpoint.ipynb</a>
Batch |<a href="https://aka.ms/azureml-infer-batch-sdk-safe-image-text-to-image-generation" target="_blank">safe-image-text-to-image-batch-endpoint.ipynb</a>

# Sample input and output

### Sample input

```json
{
   "input_data": {
        "columns": ["prompt", "image"],
        "data": [
            {
                "prompt": "Face of a yellow cat, high resolution, sitting on a park bench",
                "image": "image1",
            },
            {
                "prompt": "Face of a green cat, high resolution, sitting on a park bench",
                "image": "image2",
            }
        ],
        "index": [0, 1]
    }
}
```

> Note:
>
> - "image1" and "image2" strings are base64 format.

### Sample output

```json
[
    {
        "prompt": "Face of a yellow cat, high resolution, sitting on a park bench",
        "generated_image": "generated_image1",
        "nsfw_content_detected": null
    },
    {
        "prompt": "Face of a green cat, high resolution, sitting on a park bench",
        "generated_image": "generated_image2",
        "nsfw_content_detected": null
    }
]
```

> Note:
>
> - "generated_image1" and "generated_image2" strings are in base64 format.
> - The `stabilityai-stable-diffusion-xl-refiner-1-0` model doesn't check for the NSFW content in generated image. We highly recommend to use the model with <a href="https://learn.microsoft.com/en-us/azure/ai-services/content-safety/studio-quickstart" target="_blank">Azure AI Content Safety (AACS)</a>. Please refer sample <a href="https://aka.ms/azureml-infer-sdk-safe-image-text-to-image-generation" target="_blank">online</a> and <a href="https://aka.ms/azureml-infer-batch-sdk-safe-image-text-to-image-generation" target="_blank">batch</a> notebooks for AACS integrated deployments.

## Visualization for the prompt - "gandalf, lord of the rings, detailed, fantasy, cute, adorable, Pixar, Disney, 8k"

<img src="https://automlcesdkdataresources.blob.core.windows.net/finetuning-image-models/images/Model_Result_Visualizations(Do_not_delete)/output_gridviz_stabilityai-stable-diffusion-xl-refiner-1-0.png" alt="stabilityai-stable-diffusion-xl-refiner-1-0 input image and output visualization">
