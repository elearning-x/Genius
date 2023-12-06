# --------
# using scipy, it's kinda big but that should not be a problem
# base-notebook lacks at least numpy, widgets, so...
FROM nbhosting/scipy-notebook

# NOTES for converting a Dockerfile from nbclassic
# - explicitly pip install jupyterlab-nbhosting
#   this is what does the menu customization
# - make sure to *NOT* install anything nb-classic
#   i.e. neither of
#   jupyter_contrib_nbextensions
#   jupyter-nbextensions-configurator
#   widgetsnbextension
# - nbhosting stock image now has fewer auto-installed libs
#   so make sure to pip install these if you actually need them
#   - ipywidgets
#   - nbautoeval
#   - ipythontutor

# --------
# install librosa, resampy and wavio
RUN true \
    # the crux of the matter
 && pip install jupyterlab-nbhosting \
    # formerly known as splitcell, used a lot \
 && pip install jupyterlab-gridwidth \
    # used only once to hide one code cell in taylor \
    # formerly known as hide_input \
 && pip install jupyterlab-hidecell \
    # auto-corrected exos \
 && pip install nbautoeval \
    # auto-illustrated examples \
 && pip install ipythontutor \
 
 && apt-get update \
 && apt-get install -y gcc portaudio19-dev python3-pyaudio \
 && pip install librosa \
 && pip install resampy \
 && pip install wavio \
 && pip install sounddevice \
 && true

