# EEG-Denoise
Machine Learning methods applied to EEG Denoising for university course lab

## Overview

My goal was to try out different Machine Learning algorithms for EEG(ElectroEnchephaloGraphy) signal denoising. EEG signals are usually considered "clean" when they lack electrical signals from places other than cortical neurons, such as eye-movements(ocular signals). As such, I've built models to remove synthetically added EOG(ElectroOculography) signals 
Both Data and certain methods used were taken from the [EEGdenoiseNet Project](https://github.com/ncclabsustech/EEGdenoiseNet/tree/master).

## Data and proccessing

Single channel EEG and EOG signals were isolated and cleaned via standard methods (ICA/ICALabel) and visually checked by experts as described in the above project.

Then, I linearly mixed the signals to an SNR(Signal-to-Noise Ratio) between -7dB and 2dB, to emulate empirical data:
$$
    x = y + \lambda n
$$
x - contaminated EEG+EOG signal, y - clean EEG signal, n - EOG signal. 

$\lambda$ was calculated to fit a realistic SNR via:
$$
    SNR = 10\log\frac{RMS(x)}{RMS(\lambda\cdot n)}
$$
Where RMS is the regular Root Mean Squared measure.

Thus my Data and Target are mixed EEG+EOG and clean EEG signals respectively.

## Loss function

After trying different loss functions for signal evaluation, I stuck to one given by the Project. Relative Root Mean Squared Error in the temporal domain (RRMSE) is simply:
$$
    RRMSE_{temporal} = \frac{RMS(f(x)-y)}{RMS(y)}
$$
This gave both more believable EEG signals(for my untrained eye), and good training times.

## ML models

For this task, I wanted to compare performances of two models: 1) a simple 3 layer Neural Network, and 2) a Convolutional Neural Network.
