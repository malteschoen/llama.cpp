GPU compilation

//install libopenblas
pkg install libopenblas 
cd 
cd llama.cpp
cp /data/data/com.termux/files/usr/include/openblas/openblas_config.h .

//install required packages
pkg install ocl-icd opencl-headers opencl-clhpp clinfo

//validate that opencl works
LD_LIBRARY_PATH=/vendor/lib64:$PREFIX/lib clinfo

//install clblast
git clone https://github.com/CNugteren/CLBlast.git 
mkdir CLBlast/build 
cd CLBlast/build 
cmake .. -DBUILD_SHARED_LIBS=OFF -DTUNERS=OFF 
cmake --build . --config Release 
cmake --install . --prefix /data/data/com.termux/files/home/myLibs

//compile llamacpp
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp
cmake -B build-gpu -DLLAMA_CLBLAST=ON -DCLBlast_DIR=/data/data/com.termux/files/home/myLibs/lib/cmake/CLBlast
cd build-gpu
make -j8

//test-drive LLM
LD_LIBRARY_PATH=/vendor/lib64:$PREFIX/lib ./build-gpu/bin/main -m ./models/7B/openbuddy-zephyr-7b-v14.1-Mistral-7B-Instruct-v0.2-slerp.Q4_K_M.gguf --color -c 4096 --temp 0.7 --repeat_penalty 1.1 -n -2 --interactive-first --no-mmap -ngl 5


CPU compilation

//compile llamacpp
git clone https://github.com/ggerganov/llama.cpp.git
cd llama.cpp
cmake -B build-cpu 
cd build-cpu
make -j8

//test-drive LLM
./build-cpu/bin/main -m ./models/7B/openbuddy-zephyr-7b-v14.1-Mistral-7B-Instruct-v0.2-slerp.Q4_K_M.gguf --color -c 4096 --temp 0.7 --repeat_penalty 1.1 -n -2 --interactive-first --no-mmap


ARCHIVE

LD_LIBRARY_PATH=/vendor/lib64:$PREFIX/lib ./build/bin/main -m ./models/7B/openbuddy-zephyr-7b-v14.1-Mistral-7B-Instruct-v0.2-slerp.Q4_K_M.gguf --color -c 4096 --temp 0.7 --repeat_penalty 1.1 -n -2 --interactive-first -ngl 1


cp /data/data/com.termux/files/usr/include/openblas/openblas_config.h .


git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
mkdir build
cd build
cmake .. -DLLAMA_CLBLAST=ON -DCLBlast_DIR=/data/data/com.termux/files/home/myLibs/lib/cmake/CLBlast
cmake --build . --config Release





