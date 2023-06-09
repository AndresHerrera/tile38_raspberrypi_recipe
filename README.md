# tile38_raspberrypi_recipe

## Install tile 38 on raspberry pi

### 1.  Install  Docker 
```console
sudo apt install docker.io
```
### 2. Install tile38 from docker image: ( https://hub.docker.com/r/kirkryan/tile38 )
```console
sudo docker pull kirkryan/tile38
```

### 3. Launch tile38 server
```console
sudo docker run -p 9851:9851 kirkryan/tile38
```

![tile38 installed](s1.png)


### 4. tile38 cli for testing:
```console
sudo docker run --net=host -it kirkryan/tile38 tile38-cli
```

![tile38 cli](s2.png)

### Python client (pyle38) : ( https://github.com/iwpnd/pyle38 )
- Install pyle38 
```console
pip install pyle38
```

```console
vim test.py
```

- Example

```
import asyncio
from pyle38 import Tile38

async def main():
    tile38 = Tile38(url="redis://localhost:9851", follower_url="redis://localhost:9851")
    await tile38.set("fleet", "truck").point(52.25,13.37).exec()
    response = await tile38.follower().within("fleet").circle(52.25, 13.37, 1000).asObjects()
    assert response.ok
    print(response.dict())
    await tile38.quit()
asyncio.run(main())
```

```console
python test.py
```

![tile38 python client](s3.png)

## Tested on 

- Raspberry Pi B+ model 1.2 / https://www.raspberrypi.com/news/introducing-raspberry-pi-model-b-plus/

![raspi hardware](s4.png)

