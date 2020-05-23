# sbi-benchmark.github.io

This repository contains the source for the website [sbi-benchmark.github.io](https://sbi-benchmark.github.io), which is build on top of `mkdocs`.

The website can be run locally for development:

```commandline
$ pip install -r requirements.txt
$ mkdocs serve
```

Note that you may have to temporarily change `site_url` in `mkdocs.yml` to fetch local instead of remote assets.


To deploy the website:

```commandline
$ mkdocs gh-deploy
```


## License

MIT
