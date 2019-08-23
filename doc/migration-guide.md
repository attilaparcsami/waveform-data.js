# waveform-data.js v3.0 Migration Guide

waveform-data.js v3.0 adds multi-channel waveform data support, but introduces
a few breaking API changes from previous versions. This guide shows how to
migrate your code to the v3.0 API.

## WaveformData Attributes

Attributes such as `sample_rate`, `scale`, `length`, etc., are no longer
available via the `adapter` object, which is now private. Replace:

```javascript
const sampleRate = waveformData.adapter.sample_rate;
const scale = waveformData.adapter.scale;
const secondsPerPixel = waveformData.adapter.seconds_per_pixel;
const pixelsPerSecond = waveformData.adapter.pixels_per_second;
const length = waveformData.adapter.length;
const duration = waveformData.adapter.duration;
```

with:

```javascript
const sampleRate = waveformData.sample_rate;
const scale = waveformData.scale;
const secondsPerPixel = waveformData.seconds_per_pixel;
const pixelsPerSecond = waveformData.pixels_per_second;
const length = waveformData.length;
const duration = waveformData.duration;
```

# Waveform data min and max values

If you need an array of min or max values, replace:

```javascript
const minValues = waveformData.min;
const maxValues = waveformData.max;
```

with:

```javascript
const channel = waveformData.channel(0);
const minValues = channel.min_array();
const maxValues = channel.max_array();
```

Instead, if you want to directly access a min or max value at a specific
index, replace:

```javascript
const minValue = waveformData.min[index];
const maxValue = waveformData.max[index];
```

with:

```javascript
const channel = waveformData.channel(0);
const minValue = channel.min_sample(index);
const maxValue = channel.max_sample(index);
```

# Waveform data array indexing

The `WaveformData.at()` method, which provided access to the underlying
waveform data array, has been removed. Replace:

```javascript
const index = 20;
const minValue = waveformData.at(index);
const maxValue = waveformData.at(index + 1);
```

with:

```javascript
const channel = waveformData.channel(0);
const index = 10;
const minValue = channel.min_sample(index);
const maxValue = channel.max_sample(index);
```

## Offsets, points, and segments

The following API methods have been removed. Please use
[Peaks.js](https://github.com/bbc/peaks.js) if you want to create and manage
points and segments.

```javascript
waveformData.offset(start, end);
const start = waveformData.offset_start;
const length = waveformData.offset_length;
const duration = waveformData.offset_duration;
const inOffset = waveformData.in_offset(index);

waveformData.set_segment(start, end, id);
const segment = waveformData.segments[id];

waveformData.set_point(index, id);
const segment = waveformData.points[id];

waveformData.remove_point(id);
```