# FFT.js

- پیاده‌سازی الگوریتم‌های تبدیل فوریه سریع و معکوس همان به همراه اعداد مختلط 
- Implementation of the FFT and IFFT algorithms along with complex number


## نحوه استفاده

```js
import { fft, ifft } from "./fft.js";
import { Convert } from "./complex.js";
import crypto from "node:crypto";

const duration = 5;
const sampleRate = 44100;

function generateWhiteNoise(duration, sampleRate) {
    const whiteNoise = new Float32Array(duration * sampleRate);
    for (let i = 0; i < duration * sampleRate; i++) {
        whiteNoise[i] = crypto.randomInt(0, 10000) / 10000 * 2 - 1;
    }
    return whiteNoise;
}

const convertor = new Convert();
const whiteNoise = generateWhiteNoise(duration, sampleRate); // میتونه هر نوع نمونه سیگنالی باشه اما باید موقع تبدیل برگشت براساس ارایه مد نظر خروجی گرفته بشه که در اینجا Float32Array درنظر گرفته شده.
const xComplex = convertor.RealToComplex(whiteNoise);
const X = fft(xComplex);
for (let k = 0; k < X.length; k++) {
	const f = (k * sampleRate) / X.length;
	if (f < 0 || f > 1200) {
		X[k] = new Complex(0, 0); 
	}
}
const result = convertor.ComplexToF32(ifft(X), whiteNoise.length);

console.log(result);
```

