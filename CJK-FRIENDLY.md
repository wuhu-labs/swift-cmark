# CJK-friendly emphasis

This fork of swiftlang/swift-cmark's `gfm` branch makes the markdown-cjk-friendly flanking amendment the default for emphasis and strikethrough. It changes delimiter scanning at parse time; it does not reconstruct inlines from source-position metadata. Smart single and double quotes retain standard CommonMark flanking with the P|S punctuation predicate.

The algorithm is adapted from tats-u/qiita_marker PR #1 (`cjk_friendly_emphasis`, head 18c4791ab4604d09b42e0c510994cc6c0fe45343), under its existing BSD-compatible cmark license. The equivalent experimental swift-cmark `vgm/cjk-emphasis` branch includes a Unicode-data generator and about 8,700 added lines; this fork retains only the scanner change and compact Unicode 18 predicates. The flanking helper is shared with the strikethrough scanner. The Unicode punctuation predicate includes P and S, as CommonMark requires; upstream's old P-only table treats symbols differently: `*£*bravo.` and `**🎉**Done` remain literal with P|S, and `https://☃.net` is recognized as a GFM autolink.

Data provenance: CJK ranges from https://github.com/tats-u/markdown-cjk-friendly/blob/main/ranges.md (Unicode 18); punctuation/symbol ranges generated from https://www.unicode.org/Public/18.0.0/ucd/UnicodeData.txt, retaining general categories P* and S*. Unicode data is covered by https://www.unicode.org/license.txt. No Unicode source dataset is vendored.

Wuhu's native contract tests run all 123 upstream CJK examples and the 132 CommonMark 0.31.2 emphasis-section examples against swift-markdown's AST/HTMLFormatter, plus lazy-continuation and lossless-editor regressions. This is not a claim of full CommonMark conformance: upstream GFM HTML output deliberately flattens nested strong elements. `api_test` also has focused parser regressions.

The Wuhu dependency moves from swift-cmark 0.8.0 (`924936d0427cb25a61169739a7660230bffa6ea6`) to upstream `gfm` commit `a7ecfef49` before applying this fork's scanner/Unicode changes.
