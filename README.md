# EEG Motor Imagery Classification

I found this topic on Instagram and had no idea what I was getting into. Turns out the human brain is way more interesting than I expected.

## What this is

This project reads brain signals (EEG) and tries to figure out whether someone is imagining moving their left or right hand, without any actual movement. Just the thought.

I built this as my first neurotechnology project to learn how to work with biosignals from scratch.

## What I didn't know before building this

I had no idea the brain works in hemispheres for movement: the left hemisphere controls the right side of the body and vice versa. When you imagine moving your right hand, the left side of your brain changes its activity, not the right.

I also didn't know brain signals aren't just one wave. They're a mix of five frequency bands happening at the same time, like an equalizer. Each band is associated with a different mental state:

| Band | Frequency | What it means |
|---|---|---|
| Delta | 0.5-4 Hz | Deep sleep |
| Theta | 4-8 Hz | Drowsiness |
| Alpha | 8-13 Hz | Relaxed attention |
| Beta | 13-30 Hz | Active movement |
| Gamma | 30-40 Hz | Intense processing |

The surprising part: when you imagine a movement, beta power actually *drops* in the active hemisphere. The brain gets noisier when it's working, so the synchronized oscillation breaks down. This is called Event-Related Desynchronization (ERD) and it's the whole reason this classification is possible.

## Why I picked these three electrodes

C3, Cz and C4 sit directly over the motor cortex. C3 is over the left hemisphere, C4 over the right, and Cz is the neutral center. These three give you the asymmetry you need to distinguish left from right imagery.

## The pipeline

1. Download EEG data automatically via MNE-Python (PhysioNet dataset)
2. Filter between 1-40 Hz to remove noise and slow drift
3. Cut the recording into 4.5-second epochs around each motor imagery event
4. Extract band power from C3, Cz, C4 using Welch's method
5. Train an XGBoost classifier on 80% of the epochs
6. Evaluate with accuracy, cross-validation, and confusion matrix

## One thing I learned about feature engineering

Don't filter just to filter. You need to understand the phenomenon first. Beta band matters here because of ERD. If you didn't know that, you might throw away exactly the information the model needs.

## Results

Test accuracy: 50% (chance level). Cross-validation: 54% mean, 23% std. This is expected with only 30 epochs from 1 subject. The pipeline works, the dataset is just too small for a reliable classifier.

## Limitations

- 1 subject, 30 epochs only
- No artifact rejection (ICA not applied)
- Feature importance may reflect noise at this sample size
- Adding more subjects would improve performance significantly

## Installation

```bash
pip install mne scikit-learn xgboost pandas numpy scipy matplotlib seaborn
```

## Usage

Open and run `MotorImaginery.ipynb` in order. Data downloads automatically on first run.

