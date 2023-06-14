# CNN_RM-CRM
## Introduction
Single channel speech enhancement aims to extract a high quality intelligible speech from a noisy mixture. Recently, this task has been addressed using deep learning architectures, by either directly estimating the noise-free speech, or by predicting a mask which is applied to the noisy mixture to retrieve a clean speech signal. Masking techniques outperform direct estimation approaches, though they still suffer from distortions at lower SNRs. This is due to the phase of the signal, which plays a major role in how natural the speech sounds, however phase lacks a clear spectral structure, making it difficult to estimate.

By implicitly tackling both the magnitude and phase components of a signal, complex ratio masks have emerged as a solution for addressing the phase estimation issue. However, studies have shown that although estimates of the real mask are relatively good, the imaginary component is often underestimated [^1^]. This study examines the contribution of the imaginary mask and the effect its underestimation has on the phase of the enhanced signal. When studying the complex mask components in relation to the magnitude and phase of the enhanced signal, we found that as the imaginary mask approaches 0, the phase mask also tends towards 0, leaving the noisy phase unchanged in the recovered signal. This work aims to improve the estimation of the imaginary mask component and thereby reduce phase distortion in the enhanced audio.

## Method
To address this issue, we propose a CNN-DNN architecture for complex mask estimation. The noisy audio is divided into 20ms frames with a 50% overlap, and stacked into 480ms mini-spectrograms aimed at capturing temporal structure. A 5-layer CNN with varying filter sizes extracts features from the mini-spectrogram and the linear layers in the model estimate a complex mask associated with the central frame of the input. To avoid optimisation issues caused by large mask values, complex mask components are truncated to the range [-5;5] and compressed using a sigmoid function. After estimation, the masks are expanded back to the truncated range before being applied to the noisy speech. The truncation task, though non-invertible, restricts a small subset of mask amplitudes. Not using it limits the model's ability to learn mask patterns.

A weighted variation of the mean-squared error is proposed to address imaginary mask underestimation. The weighted loss function separates the two complex mask components, allowing the imaginary mask to have a higher contribution to the overall error. An adjustable phase error term, denoted α^PH, is introduced to improve the phase correction performed by the complex masks.

## Results
The effect of altering different combinations of α^IMAG and α^PH in the loss function is measured by evaluating the quality (PESQ) and intelligibility (ESTOI) of the enhanced speech. Results show that increasing the weight of the imaginary mask error component improves both PESQ and ESTOI scores, while emphasis on the phase error has less effect and can be detrimental when the phase weight is set too high. Thus, prioritizing the imaginary mask in model training yields complex masks better suited for speech enhancement, which also show a significant improvement from magnitude-only masks.

Code soon available

## References:
[1] Badshah, A.M., Rahim, N., Ullah, N., Ahmad, J., Muhammad, K., Lee, M.Y., Kwon, S. and Baik, S.W., 2019. Deep features-based speech emotion recognition for smart affective services. Multimedia Tools and Applications, 78(5), pp.5571-5589.

[2] Williamson, D.S., Wang, Y. and Wang, D., 2015. Complex ratio masking for monaural speech separation. IEEE/ACM transactions on audio, speech, and language processing, 24(3), pp.483-492.

[3] Hasannezhad, M., Ouyang, Z., Zhu, W.P. and Champagne, B., 2020, December. An integrated CNN-GRU framework for complex ratio mask estimation in speech enhancement. In 2020 Asia-Pacific Signal and Information Processing Association Annual Summit and Conference (APSIPA ASC) (pp. 764-768). IEEE.

[4] Yin, D., Luo, C., Xiong, Z. and Zeng, W., 2020, April. Phasen: A phase-and-harmonics-aware speech enhancement network. In Proceedings of the AAAI Conference on Artificial Intelligence (Vol. 34, No. 05, pp. 9458-9465).
