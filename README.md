# InternVL3.5-4B NPU
![Alt text](https://github.com/user-attachments/assets/6d297a34-c516-4cb1-be4a-bca471d40fa6)
<br><br>**User**:\<image\>Describe the image.<br><br>
**Answer**: The image depicts an astronaut relaxing on the moon, holding a beer bottle and sitting next to a cooler. The background shows Earth in space with stars visible above.

------------

## InternVL3.5-4B for RK3588 NPU (Rock 5, Orange Pi 5). <br/>
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)<br/><br/>
Paper: [InternVL3.5: Advancing Open-Source Multimodal Models in Versatility, Reasoning, and Efficiency](https://huggingface.co/papers/2508.18265)<br/><br/>
Hugging face: https://huggingface.co/Qwen/Qwen3-VL-4B-Instruct

------------

## Introduction

LLMs (Large Language Models) are neural networks trained on large text datasets to understand and generate language.<br>
VLMs (Vision-Language Models) add a visual encoder so the model can process images and text together.<br> 
A combined VLM+LLM system is often referred to as a multimodal model.

These models can be large—hundreds of millions to billions of parameters—which impacts accuracy, memory use, and runtime speed.<br>
On edge devices like the RK3588, available RAM and compute are limited, and even the NPU has strict constraints on supported operations.<br>
Because of this, models typically need to be quantised or simplified to fit.

Performance is usually expressed in tokens (words) per second.<br>
Once converted to RKNN, parts of the model can run on the NPU, improving speed.<br>
Despite these limits, models like Qwen3-2B run well on the RK3588 because the NPU efficiently accelerates the heavy math, and the vision encoder can be optimised. This makes advanced multimodal AI feasible on small, power-efficient devices.

------------

## Model performance benchmark (FPS)

All models, with C++ examples, can be found on the Q-engineering GitHub.<br><br>
All LLM models are quantized to **w8a8**, while the VLM vision encoders use **fp16**.<br>

| model         | RAM (GB)<sup>1</sup> | llm cold sec<sup>2</sup> | llm warm sec<sup>3</sup> | vlm cold sec<sup>2</sup> | vlm warm sec<sup>3</sup> | Resolution | Tokens/s |
| --------------| :--: | :-----: | :-----: | :--------: | :-----: | :--------:  | :--------: |
| [Qwen3-2B](https://github.com/Qengineering/Qwen3-VL-2B-NPU) | 3.1 | 21.9 | 2.6 | 10.0  | 0.9 | 448 x 448 | 11.5 |
| [Qwen3-4B](https://github.com/Qengineering/Qwen3-VL-4B-NPU) | 8.7 | 49.6 | 5.6 | 10.6  | 1.1 | 448 x 448 | 5.7 |
| [InternVL3.5-1B](https://github.com/Qengineering/InternVL3.5-1B-NPU) | 8.8 |  92 |   8.0 | 50.5    | 5.8 | 448 x 448 | 3.5 |
| [InternVL3.5-2B](https://github.com/Qengineering/InternVL3.5-2B-NPU) | 5.4 |  50 |   8.0 | 5.9    | 0.8 | 448 x 448 | 5 |
| [InternVL3.5-4B](https://github.com/Qengineering/InternVL3.5-4B-NPU) | 3.0 |  22 |   8.0 | 2.7    | 0.8 | 448 x 448 | 11.2 |
| [InternVL3.5-8B](https://github.com/Qengineering/InternVL3.5-8B-NPU) | 1.9 |  8.3 |   8.0 | 1.5    | 0.8 | 448 x 448 | 24 |
| [Qwen2.5-3B](https://github.com/Qengineering/Qwen2.5-VL-3B-NPU) | 4.8 | 48.3 |  4.0 | 17.9  | 1.8 | 392 x 392 | 7.0 |
| [Qwen2-7B](https://github.com/Qengineering/Qwen2-VL-7B-NPU) | 8.7 | 86.6 |   34.5 | 37.1  | 20.7 | 392 x 392 | 3.7 |
| [Qwen2-2.2B](https://github.com/Qengineering/Qwen2-VL-2B-NPU) | 3.3 | 29.1 |   2.5 | 17.1  | 1.7 | 392 x 392 | 12.5 |
| [InternVL3-1B](https://github.com/Qengineering/InternVL3-NPU) | 1.3 |  6.8 |   1.1 | 7.8    | 0.75 | 448 x 448 | 30 |
| [SmolVLM2-2.2B](https://github.com/Qengineering/SmolVLM2-2B-NPU) | 3.4 | 21.2 |   2.6 | 10.5   | 0.9  | 384 x 384 | 11 |
| [SmolVLM2-500M](https://github.com/Qengineering/SmolVLM2-500M-NPU) | 0.8 |  4.8 |   0.7 | 2.5    | 0.25 | 384 x 384 | 31 |
| [SmolVLM2-256M](https://github.com/Qengineering/SmolVLM2-256M-NPU) | 0.5 |  1.1 |   0.4 | 2.5    | 0.25 | 384 x 384 | 54 |

<sup>1</sup> The total used memory; LLM plus the VLM. <br>
<sup>2</sup> When an llm/vlm model is loaded for the first time from your disk to RAM or NPU, it is called a cold start.<br>
The duration depends on your OS, I/O transfer rate, and memory mapping.<br> 
<sup>3</sup> Subsequent loading (warm start) takes advantage of the already mapped data in RAM. Mostly, only a few pointers need to be restored.<br><br>
<img width="600" height="450" alt="Plot_1" src="https://github.com/user-attachments/assets/2dde8d27-c8ae-474c-b845-4ed52bdc0785" /><br>
<img width="600" height="450" alt="Plot_2" src="https://github.com/user-attachments/assets/0cf946d5-5458-4166-bc2b-fa1592ae4d6b" />

------------

## Dependencies.
To run the application, you have to:
- OpenCV 64-bit installed.
- rkllm library.
- rknn library.
- Optional: Code::Blocks. (```$ sudo apt-get install codeblocks```)

### Installing the dependencies.
Start with the usual 
```
$ sudo apt-get update 
$ sudo apt-get upgrade
$ sudo apt-get install cmake wget curl
```
#### OpenCV
To install OpenCV on your SBC, follow the Raspberry Pi 4 [guide](https://qengineering.eu/install-opencv-on-raspberry-64-os.html).<br><br>
Or, when you have no intentions to program code:
```
$ sudo apt-get install libopencv-dev 
```
------------

## Installing the app.
```
$ git clone https://github.com/Qengineering/Qwen3-VL-2B-NPU
```

#### RKLLM, RKNN
To run InternVL3, you need to have the **rkllm-runtime** library version **1.2.3** (or higher) installed, as well as the **rknpu driver** version **0.9.8**.<br>
If you don't have these on your machine, or if you have a lower version, you need to install them.<br>
We have provided the correct versions in the repo.<br>
```bash
$ cd ./Qwen3-VL-2B-NPU/aarch64/library
$ sudo cp ./*.so /usr/local/lib
$ cd ../include
$ sudo cp ./*.h /usr/local/include
```
### Download the LLM and VLM model.
The next step is downloading the two needed files (5.1 GB) from our Sync.com server:<br>
[internvl3_5-4b-instruct_w8a8_rk3588.rkllm](https://ln5.sync.com/dl/9bc3c3480#pjdftz2g-7ndjtayj-zx63rnzn-j2cd4w9j) and [internvl3_5-4b_vision_rk3588.rknn](https://ln5.sync.com/dl/0d8f223e0#zk4q9i3p-iv8spx8m-vqwvfhfq-wx2daann)<br>
Copy both into this folder.


### Building the app.
Once you have the two models, it is time to build your application.<br>
You can use **Code::Blocks**.
- Load the project file *.cbp in Code::Blocks.
- Select _Release_, not Debug.
- Compile and run with F9.
- You can alter command line arguments with _Project -> Set programs arguments..._ 

Or use **Cmake**.
```
$ mkdir build
$ cd build
$ cmake ..
$ make -j4
```
### Running the app.
The app has the following arguments.
```bash
VLM_NPU Picture RKNN_model RKLLM_model NewTokens ContextLength
```
| Argument   | Comment |
| --------------| --  |
| picture | The image. Provide a dummy if you don't want to use an image | 
| RKNN_model | The visual encoder model (vlm) | 
| RKLLM_model | The large language model (llm) | 
| NewTokens | This sets the maximum number of new tokens. Optional, default 2048| 
| ContextLength | This specifies the maximum total number of tokens the model can process. Optional, default 4096| 

<br>In the context of the Rockchip RK3588 LLM (Large Language Model) library, the parameters NewTokens and ContextLength both control different limits for text generation, and they're typical in LLM workflows.<br>
**NewTokens**<br> 
This sets the maximum number of tokens (pieces of text, typically sub-word units) that the model is allowed to generate in response to a prompt during a single inference round. For example, if set to 300, the model will not return more than 300 tokens as output, regardless of the prompt length. It's important for controlling generation length to avoid too-short or too-long responses, helping manage resource use and output size.<br>
**ContextLength**<br>
This specifies the maximum total number of tokens the model can process in one go, which includes both the prompt (input) tokens and all generated tokens. For example, if set to 2048 and your prompt already uses 500 tokens, the model can generate up to 2048-500 = 1548 new tokens. This is a hardware and architecture constraint set during model conversion and deployment, as the context window cannot exceed the model's design limit (for instance, 4096 or 8192 tokens depending on the model variant).

A typical command line can be:
```bash
./VLM_NPU ./Moon.jpg ./models/internvl3_5-4b_vision_rk3588.rknn ./models/internvl3_5-4b-instruct_w8a8_rk3588.rkllm 2048 4096
```
The NewTokens (2048) and ContextLength (4096) are optional and can be omitted.
### Using the app.
Using the application is simple. Once you provide the image and the models, you can ask everything you want.<br>
Remember, we are on a bare Rock5C, so don't expect the same quality answers as ChatGPT can provide.<br>
On the other hand, when you see the examples below, the app performs amazingly well.<br><br>
If you want to talk about the picture, you need to include the token `<image>` in your prompt once.<br>
The app remembers the dialogue until you give the token `<clear>`.<br>
With `<exit>`, you leave the application.
### C++ code.  
Below, you find the surprisingly little code of main.cpp. 
```cpp
#include "RK35llm.h"

int main(int argc, char** argv)
{
    std::string input_str;
    std::string output_str;
    RK35llm RKLLM;

    RKLLM.SetInfo(true);            //yes, you may give me additional model information
    RKLLM.SetSilence(false);        //you may print the incremental text chunks on the terminal

    if     (argc< 4) {std::cerr << "Usage: " << argv[0] << " image vlm_model llm_model [option]NewTokens [option]ContextLength\n"; return -1;}
    else if(argc==4) RKLLM.LoadModel(argv[2],argv[3]);
    else if(argc==5) RKLLM.LoadModel(argv[2],argv[3],std::atoi(argv[4]));
    else if(argc> 5) RKLLM.LoadModel(argv[2],argv[3],std::atoi(argv[4]),std::atoi(argv[5]));

    cv::Mat Pic = cv::imread(argv[1]);
    RKLLM.LoadImage(Pic);

    while(true) {
        printf("\n");
        printf("User: ");

        std::getline(std::cin, input_str);
        if (input_str == "exit") break;

        output_str = RKLLM.Ask(input_str);
//        std::cout << "\nLLM Reply: " << output_str << std::endl;
    }

    return 0;
}
```
Most code speaks for itself. One remark.<br>
The LLM generates the answer to your request in little pieces of text. You see them as if the LLM is typing on your terminal.<br>
RKLLM.SetSilence controls this behaviour. When set to `true`, it will suppress this output. RKLLM.Ask() also returns the complete answer.<br>
In main.cpp uncomment this line:
```cpp
std::cout << "\nLLM Answer: " << output_str << std::endl;
```

------------

## VLM examples.
| Image         | Dialog |
| --------------| --  |
| ![Singapore_S](https://github.com/user-attachments/assets/e742059a-3bdb-41be-9647-e7767da12ce6) |**User**: \<image\>Which city is this?<br>**Answer**: This image shows the skyline of Singapore, featuring iconic landmarks such as Marina Bay Sands and the Singapore Flyer (a giant Ferris wheel). | 
| ![ChineseWall_S](https://github.com/user-attachments/assets/3ba4b57a-4b22-4050-b995-9599c6d239de) |**User**: \<image\>Tell me something about the history of this place.<br>**Answer**: This image shows part of the Great Wall of China, an ancient series of fortifications built across several Chinese dynasties to protect against invasions and raids by nomadic groups from the Eurasian Steppe. The wall stretches over 13,000 miles (21,000 km) and was initially constructed during the Ming Dynasty in the 14th to 17th centuries. It is one of the most iconic landmarks in China and a UNESCO World Heritage Site. |
| ![Pizza_S](https://github.com/user-attachments/assets/26fd5da3-f491-4b48-978a-14eae0819247) |**User**: \<image\>What are the ingredients I need to make this?<br>**Answer**: To make a classic Margherita pizza like the one in the image, you'll need the following ingredients:<br><br>**For the Dough:**<br><br>- 2 cups of all-purpose flour<br>- 1 tablespoon of sugar<br>- 1 tablespoon of active dry yeast<br>- 1 teaspoon of salt<br>- 3/4 cup of warm water<br><br>**For the Sauce:**<br>- 1 can (28 ounces) of crushed tomatoes<br>- 2 cloves of garlic, minced<br>- 1 teaspoon of dried oregano<br>- Salt and pepper to taste<br><br>**For the Toppings:**<br>- 1 cup of fresh mozzarella cheese, sliced or torn<br>- Fresh basil leaves<br>- Olive oil for drizzling (optional)<br><br>**Instructions:**<br><br>**Making the Dough:**<br>1. In a small bowl, dissolve the yeast in warm water and let it sit for about 5 minutes until frothy.<br>2. In a large mixing bowl, combine the flour, sugar, and salt.<br>3. Add the yeast mixture to the dry ingredients and mix until a dough forms.<br>4. Turn the dough onto a floured surface and knead for about 10 minutes until smooth and elastic.<br>5. Place the dough in a lightly oiled bowl, cover it with a damp cloth, and let it rise in a warm place for about an hour or until doubled in size.<br><br>**Making the Sauce:**<br>1. In a saucepan, sauté the minced garlic in a little olive oil over medium heat until fragrant.<br>2. Add the crushed tomatoes, oregano, salt, and pepper. Simmer for about 15-20 minutes, stirring occasionally, until the sauce thickens.<br>**Assembling the Pizza:**<br>1. Preheat your oven to its highest setting (usually around 475°F or 245°C).<br>2. Punch down the risen dough and roll it out on a floured surface into a thin circle.<br>3. Transfer the rolled-out dough onto a pizza stone or baking sheet dusted with cornmeal.<br>4. Spread the tomato sauce evenly over the dough, leaving a small border for the crust.<br>5. Arrange the mozzarella cheese slices or torn pieces over the sauce.<br>6. Drizzle a little olive oil over the cheese (optional).<br>7. Scatter fresh basil leaves on top of the cheese.<br><br>**Baking:**<br>1. Bake in the preheated oven for about 10-12 minutes, or until the crust is golden brown and the cheese is bubbly and slightly browned.<br>8. Remove from the oven and let it cool for a few minutes before slicing and serving.<br><br>Enjoy your homemade Margherita pizza! |
| ![Titanic_S](https://github.com/user-attachments/assets/bf6da76c-c9a4-41c6-bea1-2b4411dd4013) |**User**: \<image\>Which movie is this?<br>**Answer**: This image appears to be from the movie "The Grand Budapest Hotel" directed by Wes Anderson. The scene shows two characters in an iconic pose, which is characteristic of the film's distinctive visual style and whimsical storytelling.|

## LLM example.
Notice the nice Markdown makeup of the answer!<br><br>
**User**: Can you make me a C++ call that embeds the Pythagorean theorem?<br><br>
**Answer**: Certainly! To create a C++ program that uses the Pythagorean theorem, we first need to understand what the Pythagorean theorem is. The theorem states that in a right-angled triangle, the square of the length of the hypotenuse (the side opposite the right angle) is equal to the sum of the squares of the lengths of the other two sides. Mathematically, this is expressed as:

\[ c^2 = a^2 + b^2 \]

where \( c \) is the length of the hypotenuse, and \( a \) and \( b \) are the lengths of the other two sides.

Here's how you can write a simple C++ program that calculates the hypotenuse using this theorem:

```cpp
#include <iostream>
#include <cmath> // For sqrt function

int main() {
    double a, b;
    
    std::cout << "Enter the length of side a: ";
    std::cin >> a;
    
    std::cout << "Enter the length of side b: ";
    std::cin >> b;

    // Calculate the hypotenuse using Pythagorean theorem
    double c = sqrt(a * a + b * b);

    std::cout << "The length of the hypotenuse is: " << c << std::endl;
    
    return 0;
}
```

### Explanation:
1. **Include Headers**: We include `<iostream>` for input and output operations, and `<cmath>` for mathematical functions like `sqrt`.

2. **Main Function**: The main function starts by declaring two variables `a` and `b` to store the lengths of the sides.

3. **Input from User**: The program prompts the user to enter the lengths of sides \( a \) and \( b \).

4. **Calculate Hypotenuse**: Using the Pythagorean theorem, we calculate the hypotenuse \( c \). We use `sqrt` function from `<cmath>` to find the square root.

5. **Output Result**: Finally, the program outputs the calculated length of the hypotenuse.

This simple program demonstrates how to embed and apply the Pythagorean theorem in a C++ application. You can compile this code using any standard C++ compiler like `g++` or `clang++`.

------------

## **[Rock5GPT](https://rock5gpt.qengineering.eu)**
To get a taste, try our professional Qwen3 AI-chatbot running on a Rock 5C: https://rock5gpt.qengineering.eu
<img width="815" height="1151" alt="Rock5GPT" src="https://github.com/user-attachments/assets/3ce5ad31-bc2b-4513-8ac9-42be793a86db" /><br>

------------

[![paypal](https://qengineering.eu/images/TipJarSmall4.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=CPZTM5BB3FCYL) 




