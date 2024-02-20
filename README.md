# Online Lecture Attention Predictor

The Online Lecture Attention Predictor is a Python tool designed to analyze online lecture videos and predict student attention spans based on blink frequency, yawn duration, and moments of concentration. This project aims to provide educators with insights into student engagement during virtual classes, allowing them to adapt their teaching strategies and improve the learning experience.

## Features

- Detection of blinks and calculation of blink frequency.
- Detection of yawns and measurement of yawn duration.
- Identification of moments of concentration based on facial muscle movements.
- Analysis of student attention trends over time for each lecture.

## Installation

1. Clone the repository to your local machine:

   ```shell
   git clone https://github.com/aditiravindranayak/online-lecture-attention-predictor.git
   ```

2. Navigate to the project directory:

   ```shell
   cd online-lecture-attention-predictor
   ```

3. Install the required Python packages:

   ```shell
   pip install -r requirements.txt
   ```

## Usage

1. Ensure you have a lecture video file (e.g., `lecture.mp4`) in the project directory.

2. Run the `main.py` script:

   ```shell
   python main.py
   ```

3. Enter the name of the lecture video file (without the extension) and choose an option (Process, Records, Exit).


## Results

The Online Lecture Attention Predictor provides educators with valuable insights into student attention spans during online lectures. By analyzing blink frequency, yawn duration, and moments of concentration, this tool helps identify patterns of engagement and disengagement. The recorded data is stored in a MySQL database for continuous assessment and improvement of teaching strategies.

## Contributing

Contributions are welcome! If you encounter any issues or have ideas for enhancing the tool, please feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
