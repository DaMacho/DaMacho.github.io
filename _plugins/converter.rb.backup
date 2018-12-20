module Jekyll
  class IPythonNotebook < Converter
    safe true
    priority :low

    def matches(ext)
      ext == ".ipynbref"
    end

    def output_ext(ext)
      ".html"
    end

    def convert(content)
      `jupyter nbconvert --config _ipynbs_profile/ipython_nbconvert_config.py --to html --template basic --stdout _ipynbs/#{content}`
    end
  end
end
