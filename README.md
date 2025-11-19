Model
Text Encoder
Model: E2ESentenceTransformer
Idea: Utilize the pre-trained sentence-transformers/paraphrase-multilingual-mpnet-base-v2 to convert textual descriptions into feature vectors.
Reason: This model demonstrates strong performance in generating high-quality text embeddings, effectively capturing semantic meaning and enabling the model to produce images aligned with the input descriptions.
Generator
Model: Generator
Idea: Takes the text embedding and a random noise vector as inputs, and generates a 64x64x3 RGB image through a series of transposed convolutional layers.
Reason: The generator is responsible for synthesizing images from the combined semantic and stochastic inputs. To enhance alignment between text and image, we fuse the text embedding with the random noise early in the network, allowing richer conditioning during image generation.
Discriminator
Model: Discriminator
Idea: Processes both the image and the text embedding through convolutional layers to distinguish real images from generated ones.
Reason: The discriminator provides feedback to the generator, helping improve the realism and fidelity of the generated images.
Adopting WGAN-GP
Reason: We employ Wasserstein GAN with Gradient Penalty (WGAN-GP) to stabilize adversarial training, mitigate mode collapse, and enhance the diversity of generated images. The gradient penalty enforces the Lipschitz constraint, leading to more stable optimization of the Wasserstein distance between real and generated image distributions.
Implementation Details:
In the Generator, the text embedding is concatenated with the random noise vector z, which is then fed through multiple transposed convolutional layers to produce the final image.
In the Discriminator, the input image is first processed by convolutional layers to extract visual features. The text embedding is reshaped to match spatial dimensions and concatenated with these visual features before final classification.
The gradient penalty significantly improves training stability and boosts both the quality and diversity of generated outputs.
3. Experiments
Data Augmentation
Applied transformations such as random cropping, horizontal flipping, and brightness adjustment to increase data diversity and improve the model’s generalization and output variation.
Hyperparameter Tuning
Experimented with different learning rates (LEARNING_RATE) and Adam optimizer settings (e.g., beta_1).
Adjusted key dimensions including noise vector size (Z_DIM), batch size (BATCH_SIZE), and the gradient penalty coefficient (LAMBDA) to optimize convergence and generation quality.
Architecture Tuning
Added more transposed convolutional layers in the Generator to enhance fine-grained image details.
Deepened the convolutional stack in the Discriminator to strengthen its ability to discriminate subtle artifacts in generated images.
Optimizer Tuning
Compared Adam with RMSprop to evaluate their impact on training dynamics and generation performance, ultimately selecting the optimizer that yielded the most stable and high-quality results.
Gradient Penalty
Integrated the gradient penalty as a core component of WGAN-GP to prevent mode collapse and ensure smooth, consistent training. This led to more diverse and realistic image generation over time.
4. Additional Notes
Pretrained Text Encoder
Leveraged a large-scale pre-trained multilingual sentence transformer for the text encoder, enabling richer semantic understanding and more accurate text-to-image alignment.
Checkpointing and Recovery
Implemented periodic model checkpointing (every few epochs) to safeguard against training interruptions and allow seamless resumption from the last saved state.
Image Generation Monitoring
Generated and saved sample images every 5 epochs to visually track the model’s progress and assess improvements in image quality and text alignment throughout training.
Testing and Evaluation
Conducted generation tests on the testing data every 1200 epochs to ensure the model generalizes well to unseen textual descriptions, not just those in the training set.
