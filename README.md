# mitmproxy
mitmproxy


## pip install mitmproxy


## mitmproxy --mode reverse:https://example.com --listen-port 8888



```python
import asyncio
from mitmproxy import http
from mitmproxy.tools.dump import DumpMaster
from mitmproxy.options import Options

class Addon:
    def start(self):
        pass

    def request(self, flow: http.HTTPFlow) -> None:
        if flow.request.pretty_host.endswith("api-clicker.pixelverse.xyz"):
            flow.request.host = "api-clicker.pixelverse.xyz"
            flow.request.port = 443
            flow.request.scheme = "https"

addons = [Addon()]

async def main():
    opts = Options(listen_port=8888, mode=["reverse:https://api-clicker.pixelverse.xyz"])
    m = DumpMaster(opts)
    m.addons.add(*addons)
    try:
        await m.run()
    except KeyboardInterrupt:
        m.shutdown()

if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    loop.run_until_complete(main())

```
