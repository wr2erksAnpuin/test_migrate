<div align="center">
<h1>AudioCore</h1>

<p>
    <a href="https://crates.io/crates/audiocore">
        <img alt="Crate Info" src="https://img.shields.io/crates/v/audiocore.svg"/>
    </a>
    <a href="https://docs.rs/audiocore/">
        <img alt="API Docs" src="https://img.shields.io/badge/docs.rs-audiocore-blue"/>
    </a>
    <a href="https://github.com/audio-tools/audiocore/actions/workflows/test.yml">
        <img src="https://github.com/audio-tools/audiocore/actions/workflows/test.yml/badge.svg" />
    </a>
    <a href="https://codecov.io/gh/audio-tools/audiocore">
        <img src="https://codecov.io/gh/audio-tools/audiocore/branch/main/graph/badge.svg" />
    </a>
</p>

<p>
    <strong>
        AudioCore is a high-performance Rust library for audio decoding and media container parsing. Supports FLAC, MP3, OGG, WAV, AAC, Opus, and WebM formats with zero-copy operations.
    </strong>
</p>

<p>
    <h3>
        <a href="https://github.com/audio-tools/audiocore/blob/main/GETTING_STARTED.md">Getting Started</a>
        <span> · </span>
        <a href="https://docs.rs/audiocore">Documentation</a>
        <span> · </span>
        <a href="https://github.com/audio-tools/audiocore/tree/main/examples">Examples</a>
        <span> · </span>
        <a href="https://github.com/audio-tools/audiocore/blob/main/BENCHMARKS.md">Performance</a>
    </h3>
</p>
</div>

---

## Key Features

* Multi-format audio decoding with seamless playback
* Media container demuxing and stream extraction  
* Comprehensive metadata and tag reading
* Automatic format detection and codec selection
* Memory-safe audio buffer manipulation
* Minimal runtime dependencies
* Optimized for embedded and server environments

Future development targets:
* WebAssembly compilation for browser applications
* C-compatible FFI bindings for language interoperability

## Supported Formats

Enable specific codecs and containers via feature flags. Default configuration includes royalty-free formats only.

### Container Formats

| Format  | Status    | Gapless | Feature   | Default | Crate                   |
|---------|-----------|---------|-----------|---------|-------------------------|
| MP4     | Stable    | No      | `mp4`     | No      | `audiocore-format-mp4`  |
| WebM    | Beta      | No      | `webm`    | Yes     | `audiocore-format-webm` |
| OGG     | Stable    | Yes     | `ogg`     | Yes     | `audiocore-format-ogg`  |
| WAV     | Production| Yes     | `wav`     | Yes     | `audiocore-format-wav`  |

### Audio Codecs

| Codec    | Status    | Gapless | Feature   | Default | Crate                 |
|----------|-----------|---------|-----------|---------|-----------------------|
| AAC      | Stable    | No      | `aac`     | No      | `audiocore-codec-aac` |
| FLAC     | Production| Yes     | `flac`    | Yes     | `audiocore-codec-flac`|
| MP3      | Production| Yes     | `mp3`     | No      | `audiocore-codec-mp3` |
| Opus     | Stable    | Yes     | `opus`    | Yes     | `audiocore-codec-opus`|
| Vorbis   | Production| Yes     | `vorbis`  | Yes     | `audiocore-codec-vorbis`|

## Quality & Performance

AudioCore prioritizes:
* Bit-perfect decoding accuracy matching reference implementations
* Robust error handling and input validation
* Comprehensive fuzz testing coverage
* Consistent, well-documented APIs

Performance benchmarks show AudioCore operates within ±10% of optimized C implementations across common codecs and CPU architectures.

## Quick Example

```rust
use audiocore::{FormatReader, Decoder};
use std::fs::File;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Open audio file
    let mut file = File::open("audio.mp3")?;
    
    // Detect format and create reader
    let mut reader = FormatReader::detect(&mut file)?;
    
    // Get default audio track
    let track = reader.default_track().unwrap();
    
    // Create decoder for the track
    let mut decoder = Decoder::new(track.codec_params())?;
    
    // Decode packets
    while let Some(packet) = reader.next_packet()? {
        let decoded = decoder.decode(&packet)?;
        // Process audio samples...
    }
    
    Ok(())
}
```

## Tools & Utilities

* `audiocore-play` - Command-line audio player with format probing
* `audiocore-check` - Validation tool for decoder output verification
* `audiocore-bench` - Performance benchmarking suite

## License

MPL-2.0 - See LICENSE file for complete terms.

## Contributing

Contributions welcome! Please review contribution guidelines and ensure all code matches project coding standards. All submissions must be compatible with MPL-2.0 licensing.
