# Model(s) Release Checklist

The Hugging Face Hub is the go-to platform for sharing machine learning models. A well-executed release can boost your model's visibility and impact. This section covers essential steps for a concise, informative, and user-friendly model release.

## ⏳ Preparing Your Model for Release

### Uploading weights

When uploading models to the hub, it's recommended to follow a set of best practices:

### Uploading Weights

- **Use separate repositories for different model weights.** For example, you can store quantization variants for the same model in a single repository, but use separate repositories for different model weights.

- **Prefer [`safetensors`](https://huggingface.co/docs/safetensors/en/index) over `pickle` for weight serialization.** `safetensors` offers improved safety and performance compared to Python's `pickle`.

### Writing a Comprehensive Model Card

A well-crafted model card (the `README.md` file in your repository) is essential for discoverability, reproducibility, and effective sharing. Your model card should include:

1. **Metadata Configuration**: The [metadata section](https://huggingface.co/docs/hub/model-cards#model-card-metadata) at the top of your model card (in YAML format) is crucial for discoverability and proper categorization. Be sure to include:
   ```yaml
   ---
   pipeline_tag: text-generation  # Specify the task
   library_name: transformers  # Specify the library
   language:
     - en  # List language for your model
   license: apache-2.0 # Specify a license
   datasets:
     - username/dataset  # List datasets used for training
   base_model: username/base-model  # If applicable
   ---
   ```

2. **Detailed Model Description**: Provide a clear explanation of what your model does, its architecture, and its intended use cases. This helps users quickly understand if your model fits their needs.

3. **Usage Examples**: Provide clear, actionable code snippets that demonstrate how to use your model for inference, fine-tuning, or other common tasks. These examples should be ready to copy and run with minimal modifications.

Bonus: You can showcase your model capabilities by placing a well structured `notebook.ipynb` in the model repository. This would allow users to directly open your notebook and run it in [Google Colab and Kaggle Notebooks](https://huggingface.co/docs/hub/en/notebooks) directly.

4. **Technical Specifications**: Include information about training parameters, hardware requirements, and any other technical details that would help users understand how to effectively use your model.

5. **Performance Metrics**: Share comprehensive benchmarks and evaluation results. Include both quantitative metrics and qualitative examples to give users a complete picture of your model's capabilities and limitations.

6. **Limitations and Biases**: Transparently document any known limitations, biases, or ethical considerations associated with your model. This helps users make informed decisions about whether and how to use your model.

### Enhancing Model Discoverability and Usability

To maximize your model's reach and usability:

1. **Library Integration**: If possible, add support for one of the many [libraries integrated with the Hugging Face Hub](https://huggingface.co/docs/hub/models-libraries) (such as Transformers or Diffusers). This integration significantly increases your model's accessibility and provides users with code snippets for working with your model.

   For example, to specify that your model works with the Transformers library:
   ```yaml
   ---
   library_name: transformers
   ---
   ```

You can also [create your own model library](https://huggingface.co/docs/hub/models-adding-libraries) or add Hub support to another existing library or codebase.

Finally, you can adopt the [Mixin class](https://huggingface.co/docs/huggingface_hub/package_reference/mixins#huggingface_hub.PyTorchModelHubMixin) when pushing custom PyTorch models.

We wrote an extensive guide on uploading best practices [here](https://huggingface.co/docs/hub/models-uploading).

Bonus: a recognised library also allows you to track downloads of your model over time.

2. **Correct Metadata**:
   - **Pipeline Tag:** Choose the correct [pipeline tag](https://huggingface.co/docs/hub/model-cards#specifying-a-task--pipelinetag-) that accurately reflects your model's primary task. This tag determines how your model appears in search results and which widgets are displayed on your model page.

   Examples of common pipeline tags:
   - `text-generation` - For language models that generate text
   - `text-to-image` - For text-to-image generation models
   - `image-text-to-text` - For vision-language models (VLMs) that generate text
   - `text-to-speech` - For models that generate audio from text
  
   - **License:** License information is crucial for users to understand how they can use the model.

3. **Research Papers**: If your model has associated research papers, you can cite them in your model card and they will be [linked automatically](https://huggingface.co/docs/hub/model-cards#linking-a-paper). This provides academic context, allows users to dive deeper into the theoretical foundations of your work, and increases citations.

   ```markdown
   ## References
   
   * [Model Paper](https://arxiv.org/abs/xxxx.xxxxx)
   ```

4. **Collections**: If you're releasing multiple related models or variants, organize them into a [collection](https://huggingface.co/docs/hub/collections). Collections help users discover related models and understand the relationships between different versions or variants.

5. **Demos**: Create a [Hugging Face Space](https://huggingface.co/docs/hub/spaces) with an interactive demo of your model. This allows users to try your model directly without writing any code, significantly lowering the barrier to adoption. You can also [link the model](https://huggingface.co/docs/hub/spaces-config-reference) from the Space to make it appear on the model page dedicated UI.

   ```markdown
   ## Demo
   
   Try this model directly in your browser: [Space Demo](https://huggingface.co/spaces/username/model-demo)
   ```
   
When you create a demo, please download the model from its repository on the Hub (instead of using external sources like Google Drive); it cross-links model artefacts and demo together and allows more paths to visibility. 

6. **Quantized Versions**: Consider uploading quantized versions of your model (e.g., in GGUF or DDUF formats) to improve accessibility for users with limited computational resources. Link these versions using the [`base_model` metadata field](https://huggingface.co/docs/hub/model-cards#specifying-a-base-model) on the quantized model cards. You can also clearly document performance differences between the original and quantized versions.

   ```yaml
   ---
   base_model: username/original-model
   base_model_relation: quantized
   ---
   ```

7. **Linking Datasets on the Model Page**: Link datasets in your `README.md` metadata to display those used directly from your model page.

   ```yaml
   ---
   datasets:
   - username/dataset
   - username/dataset-2
   ---
   ```

8. **New Model Version**: If your model is an update of an existing one, you can specify it on the older version model model card. This will [display a banner](https://huggingface.co/docs/hub/en/model-cards#specifying-a-new-version) on the older model's page linking directly to this updated version.

   ```yaml
   ---
   new_version: username/updated-model
   ---
   ```

9. **Visual Examples**: For image or video generation models, include examples directly on your model page using the [`<Gallery>` card component](https://huggingface.co/docs/hub/en/model-cards-components#the-gallery-component). Visual examples provide immediate insight into your model's capabilities.

   ```markdown
   <Gallery>
   ![Example 1](./images/example1.png)
   ![Example 2](./images/example2.png)
   </Gallery>
   ```

10. **Carbon Emissions**: If possible, specify the [carbon emissions](https://huggingface.co/docs/hub/model-cards-co2) associated with training your model. This information helps environmentally conscious users and organizations make informed decisions.

   ```yaml
   ---
   co2_eq_emissions:
     emissions: 123.45
     source: "CodeCarbon"
     training_type: "pre-training"
     geographical_location: "US-East"
     hardware_used: "8xA100 GPUs"
   ---
   ```

### Access Control and Visibility

1. **Visibility Settings**: Once everything is finalized and you're ready to share your model with the world, switch your model to public visibility in your [model settings](https://huggingface.co/docs/hub/repositories-settings). Before doing so, double-check all documentation and code examples to ensure they're accurate and complete

2. **Gated Access**: If your model requires controlled access, use the [gated access feature](https://huggingface.co/docs/hub/models-gated) and clearly specify the conditions users must meet to gain access. This is particularly important for models with potential dual-use concerns or commercial restrictions.

## 🏁 After Releasing Your Model

A successful model release extends beyond the initial publication. To maximize impact and maintain quality:

### Maintenance and Community Engagement

1. **Verify Functionality**: After release, verify that all provided code snippets work correctly by testing them in a clean environment. This ensures users can successfully implement your model without frustration.

   For example, if your model is a transformers compatible LLM, you can try the following code snippet:
   ```python
   from transformers import pipeline

   # This should work without errors
   pipe = pipeline("text-generation", model="your-username/your-model")
   result = pipe("Your test prompt")
   ```

2. **Share Share Share**: Most people discover models on social media or in internal chat channels like your company Slack or email threads, so don't hesitate to share links to your models. A good way to distribute the models is also by adding links on your website or GitHub projects. The more people visit and like your model, the higher it will go up in the trending sections of Hugging Face, leading to even more visibility!

3. **Community Interaction**: Engage with users in the Community Tab by answering questions, addressing feedback, and resolving issues quickly. Clarify confusion, adopt useful suggestions, and close off-topic discussions or pull requests to keep the space focused.

### Tracking Usage and Impact

1. **Usage Metrics**: Monitor downloads and likes to track your model's popularity and adoption. You can access total download metrics in your model settings.

2. **Monitor Contributions**: Regularly check your model tree to discover contributions made by the community. These contributions can provide valuable insights and potential collaboration opportunities.

## Enterprise Features

[Hugging Face Enterprise](https://huggingface.co/enterprise) subscription offers additional capabilities:

1. **Access Control**: Set [resource groups](https://huggingface.co/docs/hub/security-resource-groups) to control access for specific teams or users, ensuring appropriate permissions across your organization.

2. **Storage Region**: Select the data storage region (US/EU) for your model files to comply with regional data regulations and requirements.

3. **Advanced Analytics**: Use [Enterprise Analytics features](https://huggingface.co/docs/hub/enterprise-hub-analytics) to gain deeper insights into usage patterns and adoption metrics.

4. **Extended Storage**: Access additional private storage capacity to host more models and larger artifacts as your model portfolio grows.

By following these comprehensive guidelines and examples, you'll ensure your model release on Hugging Face is clear, impactful, and valuable. This will maximize the value of your work for the AI community and increase its visibility. Looking forward to your contributions!
