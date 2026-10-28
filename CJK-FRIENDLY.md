# CJK-friendly emphasis

This fork of swiftlang/swift-cmark's `gfm` branch makes the markdown-cjk-friendly flanking amendment the default for emphasis and strikethrough. It changes delimiter scanning at parse time; it does not reconstruct inlines from source-position metadata.

The algorithm is adapted from tats-u/qiita_marker PR #1 (`cjk_friendly_emphasis`, head 18c4791ab4604d09b42e0c510994cc6c0fe45343), under its existing BSD-compatible cmark license. The equivalent experimental swift-cmark `vgm/cjk-emphasis` branch includes a Unicode-data generator and about 8,700 added lines; this fork retains only the scanner change and compact Unicode 18 predicates. The flanking helper is shared with the strikethrough scanner. The Unicode punctuation predicate includes P and S, as CommonMark requires; upstream's old P-only table fails the non-BMP and currency-symbol specification cases.

Data provenance: CJK ranges from https://github.com/tats-u/markdown-cjk-friendly/blob/main/ranges.md (Unicode 18); punctuation/symbol ranges generated from https://www.unicode.org/Public/18.0.0/ucd/UnicodeData.txt, retaining general categories P* and S*. Unicode data is covered by https://www.unicode.org/license.txt. No Unicode source dataset is vendored.

Wuhu's native contract tests run all 123 upstream CJK examples and the CommonMark specification examples against this dependency, plus lazy-continuation and lossless-editor regressions. `api_test` also has focused parser regressions.
