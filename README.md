# Artificial Intelligence for clean biosignals #

[//]: # (Image References)
[image1]: ./blog-images/applewatch_1.jpg "IphoneX"
[image2]: ./blog-images/applewatch_2.jpg "Apple Girl"
[image3]: ./blog-images/biolock.png "biolock"
[image4]: ./blog-images/clean_pair.png "clean_pair"
[image5]: ./blog-images/noisy_pair.png "noisy_pair"
[image6]: ./blog-images/denoised_digits.png "denoised_digits"
[image7]: ./blog-images/noisy_ecg.png "noisy_ecg"
[image8]: ./blog-images/noisy_template.png "noisy_template"
[image9]: ./blog-images/qrs_points.png "qrs_points"
[image10]: ./blog-images/architecture.png "architecture"

*Artificial Intelligence makes wearables the powerful healthcare assistants able to predict diseases and dangerous health states in real-time. The recipe is in a continuous monitoring of the body activity with the number of embedded sensors. For consumer devices, however, obtaining the clean sensor signal which is good for any prediction becomes a major challenge itself. Sometimes it could even entirely kill the user experience of the application. At SoftServe R&D lab we use AI also for signal filtering by utilizing the power of unsupervised learning to reconstruct missing or corrupted data. In some applications, filtered signals are as good for recognition tasks as the ones recorded in perfect clinical conditions allowing for a wider range of interesting applications and use-case scenarios.*

Recent progress of Artificial Intelligence in speech, vision, pattern recognition and language understanding rapidly transforms many of the applications we face in everyday life. Embedding of predictive intelligence in consumer wearables is long being seen as a game changer for the way we care about our health and lifestyle. Machine Learning algorithms can utilize biosignals from embedded sensors to predict the dangerous health states, prevent some diseases or even alert the ambulance in case of a stroke.

![alt image][image1] ![alt image][image2]
> Major consumer electronics firms put high stakes on powering their wearables with a set of biosensors allowing to monitor in real time signals from heart rate and blood pressure to sweat level and brain waves. Image source: apple.com

### Can consumer wearables be as accurate as clinical devices ? ###
At SoftServe R&D we design and merge biomedical sensing technologies with AI predictive power and develop new healthcare, wellness and security applications for our customers. Poor signal quality is one of the major challenges of dealing with consumer biomedical sensors. Movement, external electromagnetic fields or bad sensor contacts may severely corrupt the signal, making it impossible for analysis. In consumer applications this results in inaccurate measurements and bad user experience. Hence, filtering of the signals from noise becomes as important part as a prediction and recognition itself.

The current approach to biosignal denoising is applying standard bandpass filters that pass frequencies with a certain range and rejects frequencies outside of that range. Whereas external noise is different and complex, the standard filtering does not show stable great results. As for our applications, the high accuracy is required, so we decided to search for a solution in other fields.

### Employing Neural Nets for efficient signal filtering ###

In recent years AI for computer vision showed impressive results not only in recognizing classes of images with a superhuman accuracy but also in reconstructing parts of obstructed objects in the images, colorization of black and white photos and even stylization of the picture with the strokes of famous artists. For all these the unsupervised type of learning the neural networks was used in which the algorithm gets two sets of data and is expected to figure out on its own the necessary transformation rules. At the end neural networks actually represent the very complex image filters not possible to be designed by hands. Can we use this powerful approach to remove noise from corrupted biosignal data?

![alt image][image5] ![alt image][image4]
> A Neural Network can remove complex noise background from biosignals (green vs red curves). Image source: SoftServe R&D

The idea of denoising images with the help of neural networks came way before many of recent fun examples. The architectures of the network called Denoising Autoencoders (AE) trained on a large set of pairs of noisy and clean images can then filter out noise from a new set of images. In fact, it also used for denoising the biosignals. A recent research proved that autoencoders outperform traditional filtering techniques for electrocardiogram (ECG) signals [1]. One obvious downside of this approach is that it requires pairs of corrupted and clean samples, which for some types of noise is hard to get.

![alt image][image6]
>  An example of Denoising Autoencoder in action. Neural Network trained on pairs of images effectively cleans noisy images of digits and returns clean images. Image source: SoftServe R&D

We overcame the problem of pairs preparation by utilizing a neural net architecture called Generative Adversarial Networks. Here, similarly to recent computer vision examples the system is trained on two separated datasets in an unsupervised manner. What is even more striking is that in case of ECG signal it also outperforms the Denoising Autoencoder algorithm, which already was beating the standard filtering methods.

![alt image][image10]
> Filtering with the Generative Adversarial Network. Here the pair of networks is used: Generator (our filtering network) and Discriminator (auxiliary network used during training). The filtering network receives the noisy signal and filters it. Discriminator then receives randomly “fake” (filtered) and original (clean) samples and tries to distinguish which is fake and which is real. In a long run of training the Generator will succeed in generating good enough filtered samples to fool the Discriminator network.


### Application to a real-life problem ###

![alt image][image3] ![alt image][image8]
> Filtering muscle noise from hand-recorded ECG is a challenging and important problem. SoftServe Biolock [2] uses our advanced filtering to make hand-recorded signals similar to clinically recorded ECG from the chest. Image source: SoftServe R&D

Filtering out the noise from biosensor signal appears in many of our research projects. One of the famous examples is the problem of the muscle noise in the electrocardiogram (ECG) signals. The ECG is essentially the electrical activity of the heart muscle captured by electrodes attached to the skin. If there are other muscles on the way, their activity will also be reflected in ECG, making it hard for correct analysis of shape and width of the peaks of heart beat. For clinical analysis ECG is recorded from electrodes attached to the chest and thus the muscle noise is weak, however in some of the applications it is necessary to record ECG from hands and in this case the strong muscle noise is almost unavoidable.

![alt image][image9] ![alt image][image7]
>Positions and shape of the peaks in a cardiogram signal can be well used for person identification purposes. But real signal recorded from hands looks far from being perfect. Image sources: Wikipedia, SoftServe R&D

Biometric identification from ECG signal is one application which struggles with the noise problem. It gives good results for well recorded signals, but in consumer devices the only possible way to realize it is via touching the electrodes with the fingers, as we recently did in our Biolock. Such signal is very much corrupted and practically unsuitable for recognition. We thus used it as a good test for a new filtering approaches.

| User    | Clean signal from chest | Noisy signal from hands | GAN filtered signal | AE filtered signal | SNR noisy signal (dB) | SNR filtered with GAN (dB) |
|---------|-------------------------|-------------------------|---------------------|--------------------|-----------------------|----------------------------|
| 2       | 96,67%                  | 69,46%                  | 92,04%              | 94,30%             | 5,06                  | 20,26                      |
| 34      | 98,28%                  | 91,29%                  | 99,03%              | 98,60%             | 0,77                  | 20,33                      |
| 35      | 99,57%                  | 90,75%                  | 98,71%              | 96,67%             | 3,81                  | 20,01                      |
| 52      | 99,14%                  | 71,18%                  | 97,31%              | 96,67%             | 0,80                  | 18,71                      |
| 72      | 97,31%                  | 70,54%                  | 95,38%              | 94,30%             | 4,50                  | 20,70                      |
| Average | 98,53%                  | 86,38%                  | 97,24%              | 96,82%             | 3,99                  | 21,41                      |
> Identity Recognition accuracy of SVM classifier and 1 vs 100 users setup. Signal to Noise (SNR) ratio results for noisy and filtered signals. Few identities from ECG are highlighted along with the overall average values.

After training GAN on a large dataset of clean and noisy signals we apply it to corrupted (mixed with strong muscle noise) records from PhysioNet ECG-ID database to test its quality. We then evaluate signal to noise (SNR) ratio for various types of filters as well as performance of Identification algorithms on clean, noisy and filtered datasets.

In all the cases we clearly notice the better performance of GAN over AutoEncoder architecture as well as strong signal to noise reduction (the larger number indicates lower noise power in the signal).

Efficient noise removal is important in many domains varying from background noise in speech recognition to noise from eyes blinking in electroencephalogram. New methods to reduce the unnecessary part of a signal enable a lot of new applications. While we focused on filtering the ECG signal the GAN filtering applications could clearly be applied in a much wider range of domains.



### References ###
[1] Xiong, Peng, et al. "ECG signal enhancement based on improved denoising auto-encoder." Engineering Applications of Artificial Intelligence 52 (2016): 194-202.

[2] http://www.softserveinc.com/en-us/tech/blogs/biolock-smart-identity-authentication/

[3] http://www.physionet.org/
