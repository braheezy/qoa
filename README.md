# QOA
> The [Quite OK Audio Format](https://qoaformat.org/) for Fast, Lossy Compression.

The `qoa` package is a pure Go implementation.

Decode a `.qoa` file:
```go
data, _ := os.ReadFile("groovy-tunes.qoa")
qoaMetadata, decodedData, err = qoa.Decode(inputData)
// Do stuff with decodedData
```

Or encode audio samples. This example shows a WAV file:
```go
// Read a WAV
data, _ := os.ReadFile("groovy-tunes.wav")
wavReader := bytes.NewReader(data)
wavDecoder := wav.NewDecoder(wavReader)
wavBuffer, err := wavDecoder.FullPCMBuffer()

// Figure out audio metadata and create a new QOA encoder using the info
numSamples := uint32(len(wavBuffer.Data) / wavBuffer.Format.NumChannels)
qoaFormat := qoa.NewEncoder(
  uint32(wavBuffer.Format.SampleRate),
  uint32(wavBuffer.Format.NumChannels),
  numSamples)
// Convert the audio data to int16 (QOA format)
decodedData = make([]int16, len(wavBuffer.Data))
for i, val := range wavBuffer.Data {
  decodedData[i] = int16(val)
}

// Finally, encode the audio data
qoaEncodedData, err := qoa.Encode(decodedData)
```

## Commit History
Most of this package was developed in [`goqoa`](https://github.com/braheezy/qoa) and the commit history can be found there.
