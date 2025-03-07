# Styles

This folder contains the style data of PyQtDarkTheme. If you have a better idea for this style, edit this folder and create PR.

## How to build style to qdarktheme module

You need to run the following command to add the style changes to qdarktheme.

``` sh
python -m tools.build_styles
```

When using VSCode, you can build the style resource and show the widget gallery together by running a "Check style" task.

Even if you forget to add the style changes, pre-commit or GitHub actions will automatically build and add them.

## Colors

Currently, the supporting color format is only hexadecimal notations: #RGB, #RGBA, #RRGGBB, and #RRGGBBAA.

## Theme colors

Default color maps for dark/light theme are in `colors/themes/{theme_name}.json`.

Colors that depend on specific colors like `background>textarea` adjust darkness, lightness and transparency.

If you think it's best to use additional theme colors id for more highly customizable, you need to edit `colors/themes/{theme_name}.json` and `colors/themes/validate.json` to add color id for specific widgets or components.

## Accent colors

MacOS features accent colors. qdarktheme can detect these accent colors and set the primary color to this accent color. The accent color setting file is `style/colors/os_accent.json`.

### About validate.json

`colors/themes/validate.json` is json schema for `colors/themes/{theme_name}.json`. Code completion applies to `colors/themes/{theme_name}.json` if you are using VSCode. Properties **groups**, **{color_id}.group** and **{color_id}.description** are used for automatic generation of color theme documentation.

## Icon

Currently, PyQtDarkTheme uses only SVG for styling. All SVG icons are in `svg/`. Most of the icons use Google [Material Design Icons](https://github.com/google/material-design-icons).

### Material Design Icons

All SVG icons of Material Design Icons are automatically downloaded from [material-icons](https://github.com/marella/material-icons), saved in `svg/material`, and always kept up-to-date by GitHub actions. You don't need to download icons manually.
If you want to add new material icons to this style, add the icon name and style to the `svg/material_design_icons.json` list.
> **Warning**
>
> Don't edit `svg/material` manually.

### Original Icons

PyQtDarkTheme also has own SVG icons in `svg/original`. You can add and edit its icons manually.
> **Warning**
>
> PyQtDarkTheme supports simple SVG like following code.
>
> ```svg
> <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M9 16.17 5.53 12.7a.996.996 0 1 0-1.41 1.41l4.18 4.18c.39.39 1.02.39 1.41 0L20.29 7.71a.996.996 0 1 0-1.41-1.41L9 16.17z"/></svg>
> ```
>
> Don't use properties like `fill`, `fill-opacity` and `transform` which use in qdarktheme module.

### Override Qt standard Icons

Qt has standard icons. `qdarktheme.setup_theme` can override its icons to custom icons by using QProxyStyle. You can edit custom icons in `svg/new_standard_icons.json`.

## Style Sheet

`base.qss` defines widget size/roundness, border size, padding, margin, icon, etc.

For more information about Qt Style Sheet, see the QT official site: [Qt Style Sheets](https://doc.qt.io/qt-6/stylesheet.html).

Also `base.qss` supports templates like [jinja2](https://jinja.palletsprojects.com/en/3.1.x/).

### Template examples

1. Dynamic color

    This template output the color of `background` id.

    ``` plain text
    {{ background|color }}
    ```

1. Dynamic color with child ID

    This template output the color of `primary>selection.background` id.

    ``` plain text
    {{ primary|color(state="selection.background") }}
    ```

1. Dynamic icon url

    This template output the system absolute url of `east.svg`. You can use the svg file name in the `svg/` for the id of the url.

    ``` plain text
    {{ primary|color|url(id="east") }}
    ```

1. Dynamic rotating icon url

    ``` plain text
    {{ foreground|color(state="icon")|url(id="expand_less", rotate=180) }}
    ```

1. Dynamic radius of corner

    ``` plain text
    {{ corner-shape|corner(size=2) }}
    ```

1. Dependence on system environment

    ``` plain text
    {{ |env(value="popupMode=MenuButtonPopup", version="<6.0.0", qt="PySide2") }}
    ```

For more filter information, see `qdarktheme/_filter.py`.

### QPalette

`palette.template.py` defines factory function of QPalette set theme colors for PyQtDarkTheme. This file use templates.

### Template example

Dynamic color of QPalette format

``` plain text
{{ background|color(state="popup")|palette }}
```
