# Learning Probability Density Functions

##  Objective  
The objective of this assignment is to learn an unknown probability density function (PDF) of a transformed random variable using a **Generative Adversarial Network (GAN)** .
No analytical or parametric form of the distribution is assumed. The PDF is learned only from data samples.

##  Dataset  
- **Source**: India Air Quality Data (kaggle) 
- **Feature Used**: NO2 Concentration (x)  
- **Dataset Link**: https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data  

## Methodology
1. **Data Loading**
   - The dataset is loaded using Pandas.
   - Only the NO₂ column is used for analysis.

2. **Data Cleaning**
   - Non-numeric values are converted to NaN.
   - Missing values are removed before processing.

3. **Data Transformation**
    A sinusoidal perturbation is applied to the data:
     `z = x + a_r sin(b_r x)`
     r=102303699
     ar = 0.5 * (r mod 7)
     br = 0.3 * ((r mod 5) + 1)
     
   values are:
   
  <img width="163" height="33" alt="image" src="https://github.com/user-attachments/assets/32f469b4-92ae-4454-b0f7-13085446586e" />

 5. **Learning the PDF using GAN**
     Since the analytical PDF of z is unknown, a Generative Adversarial Network (GAN) is used to learn the distribution implicitly.

    # GAN Architecture
     **Generator**
      - Input: Random noise ∼ N (0,1)
      - Fully connected neural network with LeakyReLU activations
      - Output: Generated samples 𝑧𝑓

     **Discriminator**
      - Input: Real samples z and generated samples 𝑧𝑓
      - Output: Probability of sample being real or fake
      
  * The generator tries to produce samples similar to real z, while the discriminator tries to distinguish between real and fake samples.
    
    # Training Details

   - Latent dimension: 1
   - Optimizer: Adam
   - Loss function: Binary cross-entropy
   - Epochs: 3000
   - Batch size: 64

  * Training continues until generator and discriminator losses converge near 0.69, indicating stable adversarial learning.
 5. **PDF Approximation**
    After training the GAN:
    - A large number of synthetic samples were generated from the generator.  
    - The probability density function was estimated using:
    - Histogram Density Estimation
      <img width="455" height="361" alt="image" src="https://github.com/user-attachments/assets/0f29fe61-636a-4a84-a2ad-fa8f28854408" />
    - Kernel Density Estimation (KDE)
      
      <img width="452" height="356" alt="image" src="https://github.com/user-attachments/assets/6c91c811-dd79-4cd4-833f-348c918ddae3" />
      
    - Comparison Graph
      
      <img width="551" height="373" alt="image" src="https://github.com/user-attachments/assets/e4eec93b-17ee-43b3-a446-3edb8b6ddc52" />

## Observations  

### Mode Coverage  
- The generator captures the major modes of the transformed distribution.  
- Minor mode collapse may occur due to adversarial training dynamics.  

### Training Stability  
- Training was stable after normalization.  
- Generator and discriminator losses converged gradually, indicating balanced adversarial learning.  

### Quality of Generated Distribution  
- The GAN-generated PDF closely matches the real transformed data distribution.  
- This demonstrates successful implicit learning of the unknown PDF.  

