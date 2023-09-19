# EPUB to Audiobook Converter

This project provides a command-line tool to convert EPUB ebooks into audiobooks. It uses the [Microsoft Azure Text-to-Speech API](https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-text-to-speech) to generate the audio for each chapter in the ebook. The output audio files are optimized for use with [Audiobookshelf](https://github.com/advplyr/audiobookshelf).

*This project is developed with the help of ChatGPT.*

## Audio Sample

If you're interested in hearing a sample of the audiobook generated by this tool, please [click here](https://audio.com/paudi/audio/0008-chapter-vii-agricultural-experience) to listen.

## Requirements

- Python 3.6+ Or ***Docker***
- A Microsoft Azure account with access to the [Microsoft Cognitive Services Speech Services](https://portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)

## Audiobookshelf Integration

The audiobooks generated by this project are optimized for use with [Audiobookshelf](https://github.com/advplyr/audiobookshelf). Each chapter in the EPUB file is converted into a separate MP3 file, with the chapter title extracted and included as metadata.

![demo](./examples/audiobookshelf.png)

### Chapter Titles

Parsing and extracting chapter titles from EPUB files can be challenging, as the format and structure may vary significantly between different ebooks. The script employs a simple but effective method for extracting chapter titles, which works for most EPUB files. The method involves parsing the EPUB file and looking for the `title` tag in the HTML content of each chapter. If the title tag is not present, a fallback title is generated using the first few words of the chapter text.

Please note that this approach may not work perfectly for all EPUB files, especially those with complex or unusual formatting. However, in most cases, it provides a reliable way to extract chapter titles for use in Audiobookshelf.

When you import the generated MP3 files into Audiobookshelf, the chapter titles will be displayed, making it easy to navigate between chapters and enhancing your listening experience.

## Installation

1. Clone this repository:

    ```bash
    git clone https://github.com/p0n1/epub_to_audiobook.git
    cd epub_to_audiobook
    ```

2. Create a virtual environment and activate it:

    ```bash
    python -m venv venv
    source venv/bin/activate
    ```

3. Install the required dependencies:

    ```bash
    pip install -r requirements.txt
    ```

4. Set the following environment variables with your Azure Text-to-Speech API credentials:

    ```bash
    export MS_TTS_KEY=<your_subscription_key>
    export MS_TTS_REGION=<your_region>
    ```

## Usage

To convert an EPUB ebook to an audiobook, run the following command:

```bash
python epub_to_audiobook.py <input_file> <output_folder> [--voice_name <voice_name>] [--language <language>]
```

- `<input_file>`: Path to the EPUB file.
- `<output_folder>`: Path to the output folder, where the audiobook files will be saved.
- `--voice_name`: (Optional) Voice name for the Text-to-Speech service. Default is `en-US-GuyNeural`. For Chinese ebooks, use `zh-CN-YunyeNeural`.
- `--language`: (Optional) Language for the Text-to-Speech service. Default is `en-US`.
- `--preview`: (Optional) Enable preview mode. In preview mode, the script won't convert the text to speech. Instead, it will display the chapter index and titles, allowing you to preview chapter details before processing.
- `--break_duration`: (Optional) Break duration in milliseconds between different paragraphs or sections. Default is `1250`. Acceptable values range from 0 to 5000 milliseconds.
- `--chapter_start`: (Optional) Chapter start index. Default is `1`, starting from the first chapter.
- `--chapter_end`: (Optional) Chapter end index. Default is `-1`, which means it processes up to the last chapter.

**Example**:

```bash
python epub_to_audiobook.py examples/The_Life_and_Adventures_of_Robinson_Crusoe.epub output_folder
```

Executing the above command will generate a directory named `output_folder` and save the MP3 files for each chapter inside it. Once generated, you can import these audio files into [Audiobookshelf](https://github.com/advplyr/audiobookshelf) or play them with any audio player of your choice.

## Using with Docker

This tool is available as a Docker image, making it easy to run without needing to manage Python dependencies.

First, make sure you have Docker installed on your system.

You can pull the Docker image from the GitHub Container Registry:

```bash
docker pull ghcr.io/p0n1/epub_to_audiobook:latest
```

Then, you can run the tool with the following command:

```bash
docker run --rm -v ./:/app -e MS_TTS_KEY=$MS_TTS_KEY -e MS_TTS_REGION=$MS_TTS_REGION ghcr.io/p0n1/epub_to_audiobook your_book.epub audiobook_output
```

Replace `$MS_TTS_KEY` and `$MS_TTS_REGION` with your Azure Text-to-Speech API credentials. Replace `your_book.epub` with the name of the input EPUB file, and `audiobook_output` with the name of the directory where you want to save the output files.

The `-v ./:/app` option mounts the current directory (`.`) to the `/app` directory in the Docker container. This allows the tool to read the input file and write the output files to your local file system.

## How to Get Your Azure Cognitive Service Key?

- Azure subscription - [Create one for free](https://azure.microsoft.com/free/cognitive-services)
- [Create a Speech resource](https://portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) in the Azure portal.
- Get the Speech resource key and region. After your Speech resource is deployed, select **Go to resource** to view and manage keys. For more information about Cognitive Services resources, see [Get the keys for your resource](https://learn.microsoft.com/en-us/azure/cognitive-services/cognitive-services-apis-create-account#get-the-keys-for-your-resource).

*Source: https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started-text-to-speech#prerequisites*

## Customization of Voice and Language

You can customize the voice and language used for the Text-to-Speech conversion by passing the `--voice_name` and `--language` options when running the script.

Microsoft Azure offers a range of voices and languages for the Text-to-Speech service. For a list of available options, consult the [Microsoft Azure Text-to-Speech documentation](https://learn.microsoft.com/en-us/azure/cognitive-services/speech-service/language-support?tabs=tts#text-to-speech).

You can also listen to samples of the available voices in the [Azure TTS Voice Gallery](https://aka.ms/speechstudio/voicegallery) to help you choose the best voice for your audiobook.

For example, if you want to use a British English female voice for the conversion, you can use the following command:

```bash
python epub_to_audiobook.py <input_file> <output_folder> --voice_name en-GB-LibbyNeural --language en-GB
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
