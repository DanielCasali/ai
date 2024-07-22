FROM quay.io/mgiessing/pymilvus-jupyter:v2.3.1
RUN micromamba install -y -n base altair=4 streamlit  &&     micromamba clean --all --yes
RUN pip3 install --upgrade streamlit
COPY streamlit.py /tmp/notebooks/
EXPOSE 8501
ENTRYPOINT [ "sh", "-c", "streamlit run /tmp/notebooks/streamlit.py" ]
