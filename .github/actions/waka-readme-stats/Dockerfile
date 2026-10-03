# Upstream image with anmol098/waka-readme-stats#679 applied; remove this action once it is merged
FROM wakareadmestats/waka-readme-stats:master

RUN f=/waka-readme-stats/graphics_chart_drawer.py \
  && sed -i \
    -e 's|^\(    \)years = len(yearly_data.keys())$|\1colors = {k.casefold(): v for k, v in colors.items()}\n\1years = len(yearly_data.keys())|' \
    -e 's|colors\[key\]\.get("color"|colors[key.casefold()].get("color"|' \
    "$f" \
  && grep -q 'colors = {k.casefold(): v for k, v in colors.items()}' "$f" \
  && grep -q 'colors\[key.casefold()\]' "$f"
