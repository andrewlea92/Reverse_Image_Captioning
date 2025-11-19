## 1. Generated Images from Test Descriptions  
Selected 5 descriptions from testing data and generated images using different noise vectors *z*.  

## 2. Model  
- **Text Encoder**: `sentence-transformers/paraphrase-multilingual-mpnet-base-v2` for semantic-rich embeddings.  
- **Generator**: Takes text embedding + noise *z*, outputs `64x64x3` images via transposed convolutions.  
- **Discriminator**: Fuses image features with text embedding to classify real vs. fake.  
- **WGAN-GP**: Used for stable training, reduced mode collapse, and improved diversity via gradient penalty.

## 3. Experiments  
- **Data Augmentation**: Random crop, flip, brightness.  
- **Tuning**: Learning rate, `beta_1`, `Z_DIM`, `BATCH_SIZE`, `LAMBDA`.  
- **Architecture**: Deeper generator (more detail), stronger discriminator.  
- **Optimizer**: Compared Adam vs. RMSprop; chose based on stability.  
- **Gradient Penalty**: Key to stable WGAN training.

## 4. Other Notes  
- Leveraged pretrained text encoder for better semantics.  
- Saved checkpoints every few epochs.  
- Generated samples every 5 epochs for monitoring.  
- Evaluated on unseen test descriptions every 1200 epochs.
