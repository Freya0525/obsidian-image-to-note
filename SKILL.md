---
name: obsidian-image-to-note
description: Convert supplied images, screenshots, scans, or text into Obsidian-compatible Markdown notes while preserving content hierarchy and reconstructing visible layout and styling; use when the user asks for an Obsidian note from visual or textual source material.
---

# Obsidian Image To Note

Turn the user's supplied image or text into a usable Obsidian note and save it in the directory and filename they specify.

## Output contract

- Produce a `.md` file with clean, editable Markdown whenever the source is primarily textual.
- Preserve the source's hierarchy: title, headings, paragraphs, numbered and bulleted lists, quotations, captions, footnotes, and tables.
- Reconstruct visible styling when it carries meaning. Prefer Obsidian-native Markdown, callouts, tables, and embeds; use small inline HTML/CSS or SVG only when Markdown cannot express the layout.
- Do not keep the original image by default. After conversion, compare the source and note before deciding whether an image is needed.
- Keep the original image only when the comparison finds missing/uncertain content, a material reading-order or layout discrepancy, information encoded only visually (for example an icon, diagram, handwriting, or exact color relationship), or the user explicitly asks to keep it. When kept, copy it beside the note and reference it with a relative Obsidian embed such as `![[image-name.png]]`.
- If the editable note faithfully preserves the source's text, order, structure, and meaning, remove any staged/copied original image and do not add an image embed. Content equivalence takes priority over pixel-perfect visual equivalence; simple decorative styling alone is not a reason to retain a large image.
- If exact visual fidelity cannot be achieved in editable Markdown, keep the original image as the visual source of truth, state briefly in the note which details were approximated, and explain the retention decision in the completion response.
- Use relative paths inside the vault; never embed machine-specific absolute paths in the generated note.

## Conversion decisions

1. Inspect the supplied material before writing. For images, identify text regions, reading order, tables, alignment, colors, borders, icons, and decorative elements.
2. Transcribe faithfully. Do not silently summarize, normalize numbers, or “correct” unusual wording. Mark uncertain characters as `[不确定：…]` and mention them in a short conversion note.
3. Choose the simplest format that preserves meaning: Markdown first, Obsidian callouts for highlighted blocks, HTML tables for merged cells, and SVG/HTML for genuinely diagrammatic layouts.
4. Keep the note readable in both Live Preview and Source mode. Avoid large global CSS or vault-wide changes; if styling is necessary, scope it to the note and explain where it is used.
5. Save all generated assets beside the note unless the user gives a different vault convention. Create missing directories only when the requested output path clearly authorizes it.
6. Compare the finished note against the source. Check every text region in reading order, including numbers, punctuation, headings, table cells, labels, captions, and repeated items. Also check whether color, borders, alignment, or diagrams carry meaning.
7. Apply the retention decision from the comparison: delete only the generated/copied source images that are no longer needed; never delete a user-provided file outside the output assets created for this conversion. If images are retained, verify that Markdown links point to files in the output directory. In all cases, verify that no accidental absolute local paths remain.

## Text-only input

When the user supplies text rather than an image, preserve the supplied wording and structure, then format it for Obsidian. Do not invent visual styling that was not requested; use a concise, consistent note structure.

## Completion response

Report the created note path, whether source images were retained or discarded, and any material fidelity limits or uncertain transcription. If images were retained, report their paths and why. If the user did not provide an output directory, ask for one only when there is no safe workspace destination; otherwise use the current workspace and state that choice.
