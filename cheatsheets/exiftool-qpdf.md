## exiftool and qpdf cheatsheet

### pdf
[source][002]

All metadata edits are reversible. While this would normally be considered an
advantage, it is a potential security problem because old information is never
actually deleted from the file. (However, after running ExifTool the old
information may be removed permanently using the "qpdf" utility with this
command `qpdf --linearize in.pdf out.pdf`)


[QPDF][001] — Content-preserving PDF transformation system.

---

THE END

[001](https://github.com/qpdf/qpdf "Content-preserving PDF transformation system")
[002](https://exiftool.org/TagNames/PDF.html)
