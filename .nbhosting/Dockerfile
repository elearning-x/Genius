# --------
# using scipy, it's kinda big but that should not be a problem
# base-notebook lacks at least numpy, widgets, so...
FROM nbhosting/scipy-notebook:latest

# --------
# install librosa, resampy and wavio
RUN pip install librosa
RUN pip install resampy
RUN pip install wavio
RUN pip install sounddevice

# RUN pip install PyAudio
