# Fontes Hi-Fi das telas mobile (M01..M10)

**Autor:** Pedro Lourençoni Lima
**Status:** Hi-Fi mobile baseado no blueprint ASCII (`../../Wireframes_Mobile_ASCII.md`)

Arquivos HTML+CSS editáveis que produzem os PNGs em `../../screenshots/mobile/`.

## Como regerar os PNGs

```powershell
$base = "<caminho>/sources/mobile"
$out  = "<caminho>/screenshots/mobile"
$edge = "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"
$screens = @('M01_login','M02_home_portal','M03_marcar_consulta','M04_recibos','M05_meus_dados',
             'M06_privacidade','M07_agenda_diaria','M08_iniciar_sessao','M09_anotacao_rapida','M10_encerrar_sessao')
foreach ($s in $screens) {
  $uri = "file:///" + ("$base/$s.html" -replace '\\','/')
  & $edge --headless --disable-gpu --hide-scrollbars --window-size=375,812 `
          --force-device-scale-factor=2 "--screenshot=$out/$s.png" $uri
}
```

## Como portar pro Figma do Mateus

1. Importar via plugin **html.to.design** dentro do arquivo Figma "Clínica Psicologia — Protótipo".
2. Aplicar os Local Styles do Mateus nos componentes (cores, tipografia) para alinhar com o Design System.
3. Substituir emojis por ícones SVG do Phosphor/Lucide.

## Design System usado (tokens em `_ds.css`)

- Primary: `#2563EB` · Primary-700: `#1D4ED8`
- Success: `#10B981` · Warning: `#F59E0B` · Danger: `#EF4444`
- Tipografia: Inter (fallback system-ui)
- Radius: 8/12/16/20px · Spacing: 4/8/12/16/24/32px
- Frame mobile: 375×812 (iPhone X), render a 2x (750×1624)
