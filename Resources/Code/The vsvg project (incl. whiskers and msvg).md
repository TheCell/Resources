---
tags:
  - software
  - utility
  - plotter
created: 22.04.2025
up: "[[Code]]"
---
[https://github.com/abey79/vsvg?tab=readme-ov-file](https://github.com/abey79/vsvg?tab=readme-ov-file)

## What's this?

|[_whiskers_](https://github.com/abey79/vsvg/blob/master/crates/whiskers/README.md)|[_msvg_](https://github.com/abey79/vsvg/blob/master/crates/msvg/README.md)|[_vsvg_](https://github.com/abey79/vsvg/blob/master/crates/vsvg/README.md)/[_vsvg-viewer_](https://github.com/abey79/vsvg/blob/master/crates/vsvg-viewer/README.md)|
|---|---|---|
|[![image](https://private-user-images.githubusercontent.com/49431240/270737877-77adc4ba-a47d-4997-bcd5-3a56355bbd36.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3Mzc4NzctNzdhZGM0YmEtYTQ3ZC00OTk3LWJjZDUtM2E1NjM1NWJiZDM2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBmNzI5ZGNhYWNhNTgzNTM2YTY3MWJlYmRjYTA5ZThiN2FkNWFkZGJlZmJkN2M3Y2JiOTU5YTQ4NWZkNzRlY2QmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.ID1NJq9x17IqsMS7Z8HFXCt2BaVZg3yref7tWOcTiY4)](https://private-user-images.githubusercontent.com/49431240/270737877-77adc4ba-a47d-4997-bcd5-3a56355bbd36.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3Mzc4NzctNzdhZGM0YmEtYTQ3ZC00OTk3LWJjZDUtM2E1NjM1NWJiZDM2LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTBmNzI5ZGNhYWNhNTgzNTM2YTY3MWJlYmRjYTA5ZThiN2FkNWFkZGJlZmJkN2M3Y2JiOTU5YTQ4NWZkNzRlY2QmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.ID1NJq9x17IqsMS7Z8HFXCt2BaVZg3yref7tWOcTiY4)|[![image](https://private-user-images.githubusercontent.com/49431240/270738768-54c662f6-41c1-449f-954f-5d5c33a7c25b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3Mzg3NjgtNTRjNjYyZjYtNDFjMS00NDlmLTk1NGYtNWQ1YzMzYTdjMjViLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ2ZTZhNTYzMGE3YWFkY2QwZDEwOGM2NGE0NGNmZDIxMDgzNjhlYzJhYTUzYmIyMTg2NzQxZTVlNTJmNmIyZTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.jsNz38I4upHhvpj6QeG2ftplWUXmF1ezo1EjIxe1rfA)](https://private-user-images.githubusercontent.com/49431240/270738768-54c662f6-41c1-449f-954f-5d5c33a7c25b.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3Mzg3NjgtNTRjNjYyZjYtNDFjMS00NDlmLTk1NGYtNWQ1YzMzYTdjMjViLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ2ZTZhNTYzMGE3YWFkY2QwZDEwOGM2NGE0NGNmZDIxMDgzNjhlYzJhYTUzYmIyMTg2NzQxZTVlNTJmNmIyZTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.jsNz38I4upHhvpj6QeG2ftplWUXmF1ezo1EjIxe1rfA)|[![image](https://private-user-images.githubusercontent.com/49431240/270741133-1c3d4096-9846-4902-a3f2-cc3ec43010a4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3NDExMzMtMWMzZDQwOTYtOTg0Ni00OTAyLWEzZjItY2MzZWM0MzAxMGE0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWU3NjEyYzM4MGNiZDQzZTk1YjFkNDk5NDRmYTY5NjEyNDgzMDBhMDRiNTAyYmIzYmE0MWY4YmNlZGI4Y2Q2YzMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.uvpi7g3KVmT62dPSBlPUpxD2tJ6_MFHOAZSRddahmZc)](https://private-user-images.githubusercontent.com/49431240/270741133-1c3d4096-9846-4902-a3f2-cc3ec43010a4.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDUyNzM0MTksIm5iZiI6MTc0NTI3MzExOSwicGF0aCI6Ii80OTQzMTI0MC8yNzA3NDExMzMtMWMzZDQwOTYtOTg0Ni00OTAyLWEzZjItY2MzZWM0MzAxMGE0LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA0MjElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNDIxVDIyMDUxOVomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWU3NjEyYzM4MGNiZDQzZTk1YjFkNDk5NDRmYTY5NjEyNDgzMDBhMDRiNTAyYmIzYmE0MWY4YmNlZGI4Y2Q2YzMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.uvpi7g3KVmT62dPSBlPUpxD2tJ6_MFHOAZSRddahmZc)|
|[_whiskers_](https://github.com/abey79/vsvg/blob/master/crates/whiskers/README.md) is a Rust-based, [Processing](https://processing.org)-like interactive sketching environment for generative plotter art. It's fast, it's web-ready, and it's a delight to use.  <br>  <br>Try it [here](https://bylr.info/vsvg/)!|[_msvg_](https://github.com/abey79/vsvg/blob/master/crates/msvg/README.md) is a (WIP!) fast browser for SVG collections. It smoothly addresses the challenge of browsing through large collections of generated SVGs, e.g. to find the best looking ones for plotting.|[_vsvg_](https://github.com/abey79/vsvg/blob/master/crates/vsvg/README.md) and [_vsvg-viewer_](https://github.com/abey79/vsvg/blob/master/crates/vsvg-viewer/README.md) are the core crates behind _whiskers_ and _msvg_. They implement the core data structures for manipulating vector data for plotter applications, as well as an ultra-performant, cross-platform, hardware-accelerated, and easy-to-extend viewer.|

## Documentation
The documentation is WIP—watch this space for updates.

In the meantime, each crate of the _vsvg_ project has its own README with additional information:

- [_whiskers_](https://github.com/abey79/vsvg/blob/master/crates/whiskers/README.md)
- [_whiskers-widgets_](https://github.com/abey79/vsvg/blob/master/crates/whiskers-widgets/README.md)
- [_whiskers-derive_](https://github.com/abey79/vsvg/blob/master/crates/whiskers-derive/README.md)
- [_msvg_](https://github.com/abey79/vsvg/blob/master/crates/msvg/README.md)
- [_vsvg_](https://github.com/abey79/vsvg/blob/master/crates/vsvg/README.md)
- [_vsvg-viewer_](https://github.com/abey79/vsvg/blob/master/crates/vsvg-viewer/README.md)
- [_vsvg-cli_](https://github.com/abey79/vsvg/blob/master/crates/vsvg-cli/README.md)

## Installing
There is currently no facilities to install `vsvg` unfortunately. It must be compiled and installed from source. Fortunately, this is actually not much more complicated than running a Python executable.

First, install Rust by running the command provided by [the official Rust website](https://www.rust-lang.org/tools/install):

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Normally, this will add `$HOME/.cargo/bin` in your path.

Then, download the `vsvg` source code:

```shell
git clone https://github.com/abey79/vsvg
cd vsvg
```

### Running the sketch examples
See _whiskers_'s [README.md](https://github.com/abey79/vsvg/blob/master/crates/whiskers/README.md).

### Installing _vsvg-cli_
See _vsvg-cli_'s [README.md](https://github.com/abey79/vsvg/blob/master/crates/vsvg-cli/README.md).

## Design notes
A few design considerations can be found [here](https://github.com/abey79/vsvg/issues?q=is%3Aissue+is%3Aopen+label%3Adesign-note). They concern the use of this project as basis for a possible future Rust-based `vpype-core` package.