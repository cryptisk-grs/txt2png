# txt2png

A utility which accepts a string via HTTP endpoint, and returns a PNG graphic of that text. Useful if you want to include an email address in a web page, while hiding it from (low-effort) scrapers.

![Docker Build](https://img.shields.io/docker/cloud/build/3dbrows/txt2png.svg)

## Try it out
Try out the test harness at [https://txt2png.azurewebsites.net/](https://txt2png.azurewebsites.net/). See also the [Hosting](#Hosting) section below.

## Examples
In the following examples we will use this input text: `foo@example.com`

We will output an image like this: ![Image generated by github.com/3dbrows/txt2png](https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com&background=white)

### Recommended usage: Base64 output
```
$ curl -i -H "Accept: text/plain" https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com
```
Use the returned Base64 string in HTML like this:
```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALQAAAAWCAMAAABex0blAAAABlBMVEX///8AAABVwtN+AAAAAXRSTlMAQObYZgAAAOdJREFUSMftVQkOwyAMw///9LShhjgJRwFV2oalthmhjkcMTeng4IeBN9r55QLbNQ/kV4s+LXpLvc2igeIOZZMSfhYablQNX6OZCFdKmw7J8XoVVbLWKkDUmBB+gjWNpMq/1twRg1VgCmuybuucEltZHoiY9OVI4UqEvxtE9hXELCWGtpJpOSq1zIMZmgu4KhrcaxFA/RgSnTq4LbqiHfFE+PywPToK7tijvhF5Jjg0g3LVN6IyS8yLnmhtsvDII9XqQNJ35L2eb1ctOsUoFjYi5ojIJjD1Fd7+DXwC3ygaU+05OPgvvABCiQFM5NPLBQAAAABJRU5ErkJggg==" alt="Image generated by github.com/3dbrows/txt2png" />
```

Or, use it in Markdown like this:
```markdown
![Image generated by github.com/3dbrows/txt2png](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALQAAAAWCAMAAABex0blAAAABlBMVEX///8AAABVwtN+AAAAAXRSTlMAQObYZgAAAOdJREFUSMftVQkOwyAMw///9LShhjgJRwFV2oalthmhjkcMTeng4IeBN9r55QLbNQ/kV4s+LXpLvc2igeIOZZMSfhYablQNX6OZCFdKmw7J8XoVVbLWKkDUmBB+gjWNpMq/1twRg1VgCmuybuucEltZHoiY9OVI4UqEvxtE9hXELCWGtpJpOSq1zIMZmgu4KhrcaxFA/RgSnTq4LbqiHfFE+PywPToK7tijvhF5Jjg0g3LVN6IyS8yLnmhtsvDII9XqQNJ35L2eb1ctOsUoFjYi5ojIJjD1Fd7+DXwC3ygaU+05OPgvvABCiQFM5NPLBQAAAABJRU5ErkJggg==)
```

### Base64 output with opaque white background
```
$ curl -i -H "Accept: text/plain" https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com&background=white
```
Use the returned Base64 string in HTML like this:
```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALQAAAAWCAMAAABex0blAAACJVBMVEX////m5uZUVFRBQUFFRUVERESAgICzs7M6Ojo+Pj5CQkLIyMilpaU/Pz85OTnPz8/FxcXz8/Py8vJKSkpDQ0NISEjd3d2IiIi/v79jY2M7OzsPDw9xcXH29vZ3d3cvLy81NTUzMzOUlJT5+flGRkacnJw9PT0jIyO3t7fl5eVgYGA2NjY3Nzc4ODiTk5P+/v4DAwM8PDy8vLwLCwuurq6oqKhSUlLBwcF4eHgRERFzc3MyMjJdXV1JSUn09PShoaE0NDRfX1+xsbExMTFmZmbt7e2Pj49vb297e3vQ0ND6+vqbm5v4+Ph1dXVqampcXFyvr69lZWWLi4vNzc2enp79/f1HR0fCwsLHx8fp6ens7OxLS0vg4OAFBQWrq6unp6eOjo7f398YGBj19fWEhIQXFxfDw8OYmJiKiopWVlbJycnj4+NnZ2fY2NhhYWHZ2dnAwMCGhoa+vr7T09NwcHAODg6WlpZZWVmmpqbh4eHi4uKtra2FhYWVlZWHh4ewsLCQkJAaGhq9vb3KyspbW1u7u7vr6+tMTEyNjY1YWFh+fn6RkZHu7u5kZGRra2vU1NT8/PxVVVX39/e0tLSMjIxXV1fq6urn5+eamprExMSfn592dnZOTk6JiYlycnINDQ0uLi4AAABTU1Pb29tNTU2ioqIQEBCCgoLw8PAVFRVAQEAJCQl9fX18fHwSEhIfHx8bGxuXl5ctLS3S0tLv7+9PT0+jo6MrDBGiAAACvElEQVRIx+2U6VdSQRjGX0BIrEuBKIm5pWEpRhiVxRIIRSGZaNDikmQmuVZkuZUVlZVlalqZWWaL7ZvV39fcIbkznuM9pp6jdXg+zDzM3Hne373MDEBUUf2/EghFMWLJ7PMrYqVxKxdSYBXDyBYbWriad3qNWA6KeMXCajCLDa3kn05IRI1q7bKCTlInq9XqdaxNSU1Lz1gPtM3MAtigyd7I+k05udq8zTr2JbboEyE/basB+W3bNRrlDmQKdu4yygwmjdkCu622QlmG3bFnBjSXQEq31ypTOvftJwsTYXxf2iUuAnAfKKat4CCUeDJLWWiBsgylSthROJTjBbnvMGuPHAVQZB1DrtzpraisguPVAP4TsRUABm0NBU0mEHKc9KJtWHuKKkyE8UDXBXB3up6yDTbwJAKGbmzCo80tbFvksJjPcCFnz7F15DgteB5BtwrY4QvVFDSVEFHZRdy1tVOFiTAeaGEH7jq7KIugL10OQ6cxWNZuPHnl6rXwutB1qchnv0FAOxF0Ap672UNB0wnTulUVsURhIowPuoGD5qzgNnjvSI296GfuXWJVSvm9PmzyzfcB+lNngTZT0FRCRAEDB80Vnht0XSnuBkKUdUkjD0pU3KLBngcwhPOGH7Jt10zo1kd4ewQoaDIBoOUxXgojzbjTNVCF5wbtEj9Bh2C0mLYw3DT9oEVWhw6M34ZuCvfTQjQw9gw1BZ3oFXqfz4QeF6EtWpJnoaCJBKTsFxPh4YGxfoCXr15ThXmg3fHMG7TLivA/PmqUpYfvI8IGpR1gmXyLaxYY9foJtJUH7e+U7TDy/gM6QjWeOI1a9XFcBZ8+x8jhSyMEv35DYPVi6/ck9k6ZEjFMMsP4fpAJrEyVQ38+16RR7zNNUYXJsHko9FOU1fa3i/y2+ZRaYv2L0L+0tUzfUkNEFdVy129oCLDcGqUwOQAAAABJRU5ErkJggg==" alt="Image generated by github.com/3dbrows/txt2png" />
```

Or, use it in Markdown like this:
```markdown
![Image generated by github.com/3dbrows/txt2png](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALQAAAAWCAMAAABex0blAAACJVBMVEX////m5uZUVFRBQUFFRUVERESAgICzs7M6Ojo+Pj5CQkLIyMilpaU/Pz85OTnPz8/FxcXz8/Py8vJKSkpDQ0NISEjd3d2IiIi/v79jY2M7OzsPDw9xcXH29vZ3d3cvLy81NTUzMzOUlJT5+flGRkacnJw9PT0jIyO3t7fl5eVgYGA2NjY3Nzc4ODiTk5P+/v4DAwM8PDy8vLwLCwuurq6oqKhSUlLBwcF4eHgRERFzc3MyMjJdXV1JSUn09PShoaE0NDRfX1+xsbExMTFmZmbt7e2Pj49vb297e3vQ0ND6+vqbm5v4+Ph1dXVqampcXFyvr69lZWWLi4vNzc2enp79/f1HR0fCwsLHx8fp6ens7OxLS0vg4OAFBQWrq6unp6eOjo7f398YGBj19fWEhIQXFxfDw8OYmJiKiopWVlbJycnj4+NnZ2fY2NhhYWHZ2dnAwMCGhoa+vr7T09NwcHAODg6WlpZZWVmmpqbh4eHi4uKtra2FhYWVlZWHh4ewsLCQkJAaGhq9vb3KyspbW1u7u7vr6+tMTEyNjY1YWFh+fn6RkZHu7u5kZGRra2vU1NT8/PxVVVX39/e0tLSMjIxXV1fq6urn5+eamprExMSfn592dnZOTk6JiYlycnINDQ0uLi4AAABTU1Pb29tNTU2ioqIQEBCCgoLw8PAVFRVAQEAJCQl9fX18fHwSEhIfHx8bGxuXl5ctLS3S0tLv7+9PT0+jo6MrDBGiAAACvElEQVRIx+2U6VdSQRjGX0BIrEuBKIm5pWEpRhiVxRIIRSGZaNDikmQmuVZkuZUVlZVlalqZWWaL7ZvV39fcIbkznuM9pp6jdXg+zDzM3Hne373MDEBUUf2/EghFMWLJ7PMrYqVxKxdSYBXDyBYbWriad3qNWA6KeMXCajCLDa3kn05IRI1q7bKCTlInq9XqdaxNSU1Lz1gPtM3MAtigyd7I+k05udq8zTr2JbboEyE/basB+W3bNRrlDmQKdu4yygwmjdkCu622QlmG3bFnBjSXQEq31ypTOvftJwsTYXxf2iUuAnAfKKat4CCUeDJLWWiBsgylSthROJTjBbnvMGuPHAVQZB1DrtzpraisguPVAP4TsRUABm0NBU0mEHKc9KJtWHuKKkyE8UDXBXB3up6yDTbwJAKGbmzCo80tbFvksJjPcCFnz7F15DgteB5BtwrY4QvVFDSVEFHZRdy1tVOFiTAeaGEH7jq7KIugL10OQ6cxWNZuPHnl6rXwutB1qchnv0FAOxF0Ap672UNB0wnTulUVsURhIowPuoGD5qzgNnjvSI296GfuXWJVSvm9PmzyzfcB+lNngTZT0FRCRAEDB80Vnht0XSnuBkKUdUkjD0pU3KLBngcwhPOGH7Jt10zo1kd4ewQoaDIBoOUxXgojzbjTNVCF5wbtEj9Bh2C0mLYw3DT9oEVWhw6M34ZuCvfTQjQw9gw1BZ3oFXqfz4QeF6EtWpJnoaCJBKTsFxPh4YGxfoCXr15ThXmg3fHMG7TLivA/PmqUpYfvI8IGpR1gmXyLaxYY9foJtJUH7e+U7TDy/gM6QjWeOI1a9XFcBZ8+x8jhSyMEv35DYPVi6/ck9k6ZEjFMMsP4fpAJrEyVQ38+16RR7zNNUYXJsHko9FOU1fa3i/y2+ZRaYv2L0L+0tUzfUkNEFdVy129oCLDcGqUwOQAAAABJRU5ErkJggg==)
```

### Binary output
```
$ curl -i https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com&background=white
```
(If you want, you can optionally specify a header of `Accept: image/png`.)

HTML:
```html
<img src="https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com&background=white" />
```

Markdown:
```markdown
![alt text](https://txt2png.azurewebsites.net/txt2png?input=foo%40example.com&background=white)
```

In this example, the input text is within your page source, which offers no privacy at all; so don't use this method if you are trying to conceal anything from scrapers.

## Query string parameters
Name | Restrictions
--- | ---
`input` | Required. The text to render. Default length restriction is `0 < length <= 280`. Must contain non-whitespace characters.
`background` | Optional. Specify `&background=white` for an opaque white background. Omit the parameter for a transparent background.

## Important notes
Don't forget to consider accessibility concerns e.g. how will a blind person's screen reader react to the image? Use meaningful alt-text in the `img` tag.

Be advised that any attempts to obtain security or privacy through obscurity are ineffective against any sufficiently determined attacker. This tool merely raises the threshold of effort required to harvest mildly sensitive data (such as email addresses). Please do not confuse this tool for any form of cryptography.

Please note that GitHub does not support the display of embedded Base64 images in Markdown (using `data:image/png;base64,...`).

## Hosting
A hosted version is available here: [https://txt2png.azurewebsites.net/](https://txt2png.azurewebsites.net/) (maximum input length per image: 280 characters). The hosted service is provided by a hobbyist, and is free for reasonable private use.

I, the author, certify that I do not keep any logs of requests made to the hosted service, and that I release all copyright claims to images generated using it.

For commerical use, please host this yourself and consider making a donation: [![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/Q5Q71TISU)

A Docker image is available on DockerHub at [3dbrows/txt2png](https://hub.docker.com/r/3dbrows/txt2png). Example use:
```
docker run -p "8080:80" -e Input__MaxLength=80 -e Logging__LogLevel__Microsoft=None 3dbrows/txt2png
```
The optional flag `-e Input__MaxLength=80` (N.B. **two** underscores) sets the max input length to `80` chars instead of the default `280`. The optional flag `-e Logging__LogLevel__Microsoft=None` turns off logging (also two underscores).

When running locally, you can access the test harness in your browser at `http://localhost:8080` and make requests like:
```
curl -i -H "Accept: text/plain" http://localhost:8080/txt2png?input=foo%40example.com
```

## Licencing and Contributing
Please see [LICENSE](LICENSE) and [CONTRIBUTING.md](CONTRIBUTING.md).