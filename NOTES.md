# Endpoint oficial público de Microsoft

Además del scraping a `bing.gifposter.com` que usa este proyecto, Microsoft expone
un endpoint público (no documentado oficialmente, pero estable y usado por la
mayoría de las apps de "Bing wallpaper") para obtener el wallpaper diario:

```
https://www.bing.com/HPImageArchive.aspx?format=js&idx=0&n=8&mkt=en-US
```

- `idx`: desplazamiento en días hacia atrás desde hoy (0 = hoy).
- `n`: cantidad de días a devolver (máximo ~8, Microsoft solo retiene los últimos
  días).
- `mkt`: mercado/idioma (ej. `en-US`).

La respuesta es JSON y cada entrada trae un `urlbase`, por ejemplo:

```
/th?id=OHR.LavenderRows_EN-US2831933520
```

Al `urlbase` se le agrega `https://www.bing.com` + el sufijo de resolución
deseado + `.jpg` para obtener la imagen directamente.

## Resoluciones soportadas

Verificadas contra el endpoint real (todas devuelven `200 OK` con una imagen
válida):

**Escritorio (horizontal)**
- `UHD` (equivale a 3840x2160, la máxima calidad real disponible)
- `1920x1200`
- `1920x1080`
- `1366x768`
- `1280x768`
- `1280x720`
- `1024x768`
- `800x480`
- `640x480`

**Móvil (vertical)**
- `1080x1920`
- `768x1280`
- `720x1280`
- `480x800`
- `400x240`
- `320x240`
- `240x320`

No existen sufijos intermedios como `2560x1440` o `3840x2160` explícito (dan
`404`) — para 4K real hay que pedir el alias `UHD`.

## Limitación frente al scraping actual

Este endpoint solo mantiene el historial de los **últimos ~7-8 días**, mientras
que `bing.gifposter.com` tiene un archivo histórico de miles de wallpapers. Por
eso el proyecto sigue usando el scraping, pero si en algún momento se busca
mayor confiabilidad (JSON en vez de parsear HTML) o resolución UHD real, este
es el camino recomendado.
