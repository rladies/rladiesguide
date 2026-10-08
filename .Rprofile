options(blogdown.method = 'markdown')

# Netlify only accepts the Hugo pin as an environment variable, so netlify.toml
# is the single source for it and everything else reads the version from there.
local({
  if (!file.exists("netlify.toml")) return(invisible(NULL))
  pin <- grepv("^\\s*HUGO_VERSION", readLines("netlify.toml", warn = FALSE))
  if (length(pin)) {
    options(blogdown.hugo.version = sub('.*"([^"]+)".*', "\\1", pin[[1]]))
  }
})
