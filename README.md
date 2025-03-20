# loudness_ebur128

## Introduction

This library is an efficient C++ implementation of tools to measure the loudness
of audio according to European Broadcasting Union Recommendation 128
([EBU R 128](https://tech.ebu.ch/publications/r128/)).

## Build instructions

### Prerequisites

This project requires Bazel to build. See
[Bazel getting started](https://bazel.build/start) for installation
instructions.

### Build

It is recommended to build this project with optimizations turned on.

```
bazel build -c opt ...
```

## Loudness measurement usage example

This example uses `loudness::EbuR128Analyzer` to measure the loudness of a
stream of audio. Audio may be streamed into the analyzer in chunks. The
interface is highly flexible and supports measuring multi-channel PCM in a
variety of formats.

### Construct the analyzer

```cpp
// Configure details about the number of channels and sample rate of the audio
// being analyzed.
int32_t num_channels = ...;
int32_t sample_rate_hz = ...;
// Optionally enable measuring the true peak.
bool enable_true_peak_measurement = ...;

// Set the channel weights, which must agree with the order in which PCM samples
// are passed in later.
// If channels will be passed in [L, R, C, LFE, Ls, Rs] order it is OK to use
// `loudness::DefaultChannelWeights()`
std::vector<float> weights_5dot1 = loudness::DefaultChannelWeights();
std::vector<float> weights_stereo = loudness::DefaultChannelWeights();
// Or to use a custom order or layout, arrange weights based on the channel type
// in ITU 1770-4 tables 4 and 5.
std::vector<float> weights_7dot1dot4 =  {1.0f, 1.0f, 1.0f, 0.0f, 1.41f, 1.41f,
                                         1.0f, 1.0f, 1.0f, 1.0f, 1.0f, 1.0f}

// Create the `EbuR128Analyzer`.
loudness::EbuR128Analyzer ebur_r128_analyzer(num_channels, weights_5dot1,
                                             sample_rate_hz,
                                             enable_true_peak_measurement);

// The library is flexible and supports a variety of input formats. See
// `SampleFormat` and `SampleLayout` in `ebur128_analyzer.h` for details.
loudness::SampleFormat sample_format = ...;
loudness::SampleLayout sample_layout = ...;
```

### Stream audio to the analyzer

```cpp
for each chunk of audio:
  // Raw PCM, format and arrangement is based on `sample_format` and
  // `sample_layout`.
  void* pcm_audio_data = ...;
  // The number of samples per channel in this chunk.
  int64_t num_samples_per_channel = ...;

  // Feed it the analyzer.
  ebur_r128_analyzer.process(pcm_audio_data, number_of_samples_per_channel,
                             sample_format, sample_layout);
```

### Retrieve data from the analyzer

```cpp
// Query the overall loudness value. This may also be done while streaming to
// see intermediate values. There are various properties that can be queried.

float digital_peak_dbfs = ebu_r128_analyzer.digital_peak_dbfs();
float true_peak_dbfs = ebu_r128_analyzer.true_peak_dbfs();
// Some values are held in optionals, in particular they may not be available
// for short sequences.
std::optional<float> integrated_loudness =
ebu_r128_analyzer.GetRelativeGatedIntegratedLoudness();
std::optional<loudness::EbuR128Analyzer::LRAStats> lra_stats =
ebu_r128_analyzer.GetLoudnessRangeStats();
```

## License

Released under the Apache 2.0 License. See [LICENSE](LICENSE) for details.
