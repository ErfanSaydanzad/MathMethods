# Visualization Scripts

Put chapter-level interactive scripts in this folder.

Use filenames that match the generated chapter slug:

- `chapter-04.js`
- `chapter-05.js`
- `chapter-07.js`

After you rebuild the website, a chapter automatically loads its matching script if the file exists.

Example:

```js
window.LectureNotes.registerInteractiveFigure("demo-id", async (mount, LectureNotes) => {
  await LectureNotes.loadScript("https://cdn.plot.ly/plotly-2.35.2.min.js");
  mount.innerHTML = "";
  Plotly.newPlot(mount, [{ x: [0, 1, 2], y: [0, 1, 4] }]);
});
```
