---
id: 99
title: 2. Konvence
date: 2014-01-15T10:53:27+01:00
author: Martin Maly
layout: page
guid: http://retrocip.uelectronics.info/?page_id=99
dsq_thread_id:
  - "2132313054"
---
Assembler rozlišuje typ procesoru podle přípony:

| Přípona | Procesor          |
| ------- | ----------------- |
| .a80    | Intel 8080 / 8085 |
| .z80    | Zilog Z80         |
| .a65    | MOS 6502          |

Pokud nepozná typ podle přípony, zkouší najít pseudoinstrukci „.cpu“

Po úspěšném překladu souboru filename.ext (např. pokus.a80) vzniknou soubory filename.ext.hex a filename.ext.lst (pokus.a80.hex a pokus.a80.lst), které obsahují přeložený kód ve formátu INTEL HEX a listing překladu.

Assembler nerozlišuje mezi velkými a malými písmeny, interně si názvy instrukcí i návěští překládá na velká písmena. Název návěští smí obsahovat znaky A-Z, 0-9, – a _.