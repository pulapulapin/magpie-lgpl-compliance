# Magpie — FFmpeg / LAME LGPL compliance materials

This repository publishes **only library-level** LGPL compliance materials for
the media components shipped inside Magpie. It does not contain, and does not
publish, the Magpie application source code.

## Scope

- FFmpeg 7.1.5 (libavformat, libavcodec, libavutil; macOS shared, iOS static)
- LAME 4.0 (libmp3lame; macOS shared, iOS static)
- Pinned official source archives, reproducible build scripts, license texts,
  static archives, extracted object files, headers and build manifests.
- Built with `--disable-gpl --disable-nonfree --disable-version3`; no GPL,
  nonfree or version3 components.

## Fixed download location

Release `compliance-ffmpeg-7.1.5`:

- Tag page:
  <https://github.com/pulapulapin/magpie-lgpl-compliance/releases/tag/compliance-ffmpeg-7.1.5>
- Archive:
  <https://github.com/pulapulapin/magpie-lgpl-compliance/releases/download/compliance-ffmpeg-7.1.5/magpie-lgpl-corresponding-source-7.1.5.tar.xz>

The archive contains the corresponding source, build scripts, license texts,
iOS relinkable objects (device and simulator), and `SHA256SUMS` covering every
file. `SHA256SUMS` in this repository records the archive hash itself.

## Relinkable objects (iOS static linking)

The archive includes, for `iphoneos` and `iphonesimulator`:

- `libavformat.a`, `libavcodec.a`, `libavutil.a`, `libmp3lame.a`
- extracted object files under `ios-relink/<platform>/objects/`
- public headers, `build-manifest.json`, `OBJECTS.txt` and the exact
  `OTHER_LDFLAGS` used by the shipped Runner target.

If these library objects are insufficient to relink Runner itself, the
application-side relink kit (object files only, **no source**) can be requested
under the written offer in [WRITTEN-OFFER.md](WRITTEN-OFFER.md):

- Email `839200644@qq.com`, subject `LGPL relink request`, stating the product
  channel and version.
- Delivery within 30 days, by download link or physical media at no more than
  the cost of distribution.
- The offer is valid for three years from the release date of the binary.

## Notes

- The application source is outside the compliance scope.
- No certificate, provisioning profile or private key is published here.
- The contact address is currently a personal mailbox; when a project-domain
  mailbox is available, this file, the release notes and the in-app notices
  will be updated together, with forwarding kept for the old address.
