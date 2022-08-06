# Meowify
Turn vocals from any youtube song to meows!

## This is a fork 🍴
To run the code, install:

1. `python` >= 3.7, < 3.9
2. `poetry` from https://python-poetry.org/docs/master#installation
3. python dependencies:

    ```bash
    # enter the local virtual environment
    poetry shell
    
    # install incompatible colab
    pip install google-colab
    
    # sync other dependencies
    poetry install -vv
    ```
4. `ffmpeg` e.g. with Ubuntu

    ```bash
    sudo apt install ffmpeg
    ```

> The `poetry` commands _may_ be replaced by
> ```
> pip install -r requirements.lock
> ```
> But this is untested.

The original `requirements.txt` cannot be resolved on my machine. 
This issue turns out to be a great opportunity to understand the python dependency hell and its resolution. 

To run the code I rewrite the dependencies in `pyproject.toml`, and use `poetry` to resolve the dependencies. Apparently there were some conflicts, and I was able to manually tweak the versions to generate a consistent environment, written in the locked versions file `poetry.lock`. 

> Side note: I actaully tried `pyenv` first, as it seems to be more light weight than `poetry`, but it constantly fails to resolve the dependencies with no useful error messages. Poetry with `-vv` (verbose output) is perfect. It prints out the resolution in an elegant, human readable way, which makes it super easy to fix the conflicts. 

I then proceed to run the code with `flask run`. However it turns out that there are several API breaks due to updates of dependent packages. These can be fixed by googling and adding extra version constraints to `pyproject.toml`. 

One big issue comes from `google-colab`. It is required by `ddsp.colab.colab_utils`, which is in turn required by `ddsp_timbre_transfer.py`. I understand that this is some API provided by the google colab platform, but the `google-colab` package on PyPI is some weird, old package with ancient dependencies that cannot work with the rest of the dependencies. In fact `meowify` doesn't really depend on colab, but some refactoring is required before it can be removed from the dependency list, which I am too lazy to pull through. To just make the code work, I simply install colab seperately with `pip`. 

There remain some minor issues, e.g. older versions of `youtube-dl` no longer work so it has to be updated. Also, `ffmpeg` is required, which can be easily installed with the system package manager. Now we have a working meowifier! 😸

Original README:

---

The project was developed for [HAMR 2020](https://www.ismir2020.net/hamr/) in 48 hours and received the **Best Hack/Research Direction** award.

![alt text](https://github.com/gulnazaki/meowify/blob/main/index.png?raw=true)

## About
This is an initial release using the Flask microframework. Eventually, I aim to deploy it on [heroku](https://www.heroku.com).

I wouldn't recommend running it without cuda, since ` ddsp.training.metrics.compute_audio_features` will take too long,
especially for long tracks.

DISCLAIMER: Thanks and credits to Magenta for providing [DDSP](https://github.com/magenta/ddsp) and the colab notebooks that I used and modified
for local use and Deezer for providing [Spleeter](https://github.com/deezer/spleeter).

Here is an example for ["Dumb" by Nirvana](https://www.youtube.com/watch?v=8xiwuumLkOQ)

## Installation
```
git clone https://github.com/gulnazaki/meowify.git
cd meowify
pip3 install -r requirements.txt
```
## Running
```
flask run
```
visit [localhost:5000](localhost:5000), enter a youtube link and have fun
