## 问题
warning: Debugger speedups using cython not found.

#### 解决方法
```
python /Applications/PyCharm.app/Contents/helpers/pydev/setup_cython.py  build_ext --inplace
```

```
jinlong  (e) om_news_spider  ~  python /Applications/PyCharm.app/Contents/helpers/pydev/setup_cython.py  build_ext --inplace
running build_ext
building '_pydevd_bundle.pydevd_cython' extension
creating build
creating build/temp.macosx-10.12-x86_64-3.6
creating build/temp.macosx-10.12-x86_64-3.6/_pydevd_bundle
clang -Wno-unused-result -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/usr/local/Cellar/python3/3.6.2/Frameworks/Python.framework/Versions/3.6/include/python3.6m -c _pydevd_bundle/pydevd_cython.c -o build/temp.macosx-10.12-x86_64-3.6/_pydevd_bundle/pydevd_cython.o
creating build/lib.macosx-10.12-x86_64-3.6
creating build/lib.macosx-10.12-x86_64-3.6/_pydevd_bundle
clang -bundle -undefined dynamic_lookup build/temp.macosx-10.12-x86_64-3.6/_pydevd_bundle/pydevd_cython.o -o build/lib.macosx-10.12-x86_64-3.6/_pydevd_bundle/pydevd_cython.cpython-36m-darwin.so
copying build/lib.macosx-10.12-x86_64-3.6/_pydevd_bundle/pydevd_cython.cpython-36m-darwin.so -> _pydevd_bundle
running build_ext
building '_pydevd_frame_eval.pydevd_frame_evaluator' extension
creating build/temp.macosx-10.12-x86_64-3.6/_pydevd_frame_eval
clang -Wno-unused-result -Wsign-compare -Wunreachable-code -fno-common -dynamic -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes -I/usr/local/Cellar/python3/3.6.2/Frameworks/Python.framework/Versions/3.6/include/python3.6m -c _pydevd_frame_eval/pydevd_frame_evaluator.c -o build/temp.macosx-10.12-x86_64-3.6/_pydevd_frame_eval/pydevd_frame_evaluator.o
creating build/lib.macosx-10.12-x86_64-3.6/_pydevd_frame_eval
clang -bundle -undefined dynamic_lookup build/temp.macosx-10.12-x86_64-3.6/_pydevd_frame_eval/pydevd_frame_evaluator.o -o build/lib.macosx-10.12-x86_64-3.6/_pydevd_frame_eval/pydevd_frame_evaluator.cpython-36m-darwin.so
copying build/lib.macosx-10.12-x86_64-3.6/_pydevd_frame_eval/pydevd_frame_evaluator.cpython-36m-darwin.so -> _pydevd_frame_eval
```