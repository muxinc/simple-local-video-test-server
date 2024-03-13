# simple-local-video-test-server
Uses a simple static server to host media files (with expected mimetype mappings).

# environment dependencies

- `node`
- `npm`

# setup

1. clone repository (e.g. `git clone https://github.com/muxinc/simple-local-video-test-server.git`)
1. install npm package (`npm i`)

# starting server

- `npm run start`

You can validate the server is running by opening the URL listed in `Available on:` after starting in Safari. The entire `/public` directory should be navigable via the browser. From there, you can try loading a particular media playlist, e.g.
`/mux-tests/session-data/multivariant-with-mux-other-session-data.m3u8`