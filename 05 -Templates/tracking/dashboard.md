# 📊 Heatmap

```dataviewjs
const data = {
    intensityScaleStart: 1,
    intensityScaleEnd: 5,
    colors: {
        green: [
            "#ebedf0",
            "#c6e48b",
            "#7bc96f",
            "#239a3b",
            "#196127"
        ]
    },
    entries: []
}

for(let page of dv.pages('"05 -Templates/tracking/daily"')){
    if(page.habito){
        data.entries.push({
            date: page.file.name,
            intensity: Number(page.habito)
            // 👇 sin content → sin texto dentro
        })
    }
}

renderHeatmapCalendar(this.container, data)
```