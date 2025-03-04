# .smi-to-.ass
.smi to .ass for Mac.  support 한국어/eng/Deutsch/Русский



made by gemini, ChatGPT, microsoft copilot perplexity, ...etc.

---

1. 소개

이 Python 프로그램은 SMI (Synchronized Multimedia Integration Language) 자막 파일을 가라오케 스타일 자막 표시 형식으로 변환합니다. 오디오와 동기화된 자막의 동적 색상 변경을 통해 현재 발음되는 음절(또는 선택적으로 개별 자모)을 강조 표시합니다. 주요 목표는 SMI 자막을 지원하지만 제한이 있을 수 있는 IINA와 같은 비디오 플레이어에서 기본적인 가라오케 효과를 사용할 수 있도록 하는 것입니다.

2. 기능

SMI 파싱: 프로그램은 SMI 파일을 파싱하여 각 자막 블록의 타이밍 정보(<SYNC> 태그)와 텍스트 내용을 추출합니다.
인코딩 감지: 다양한 문자 집합을 올바르게 처리하기 위해 SMI 파일의 인코딩(UTF-8, EUC-KR, Shift-JIS 등)을 자동으로 감지하려고 시도합니다.
HTML 엔터티 처리: &nbsp; (줄 바꿈 없는 공백), &lt; (보다 작음), &gt; (보다 큼) 및 &amp; (앰퍼샌드)와 같은 HTML 엔터티를 올바르게 처리합니다.
가라오케 효과 (음절 기반):
핵심 기능은 각 음절이 발음될 때 색상을 변경하여 가라오케 효과를 적용하는 것입니다.
색상 변경은 SMI 파일의 <SYNC> 태그에 지정된 Start 시간을 기반으로 합니다.
프로그램은 <font color="..."> 태그를 사용하여 이 효과를 구현하고 각 음절을 적절한 색상 태그로 래핑합니다.
가라오케 효과(자모 기반, 개념적):
프로그램은 개별 자모(한글의 기본 구성 요소) 수준에서 더 세분화된 가라오케 효과를 개념적으로 지원할 수 있습니다.
이를 위해 jamo 라이브러리를 사용하여 음절을 구성 자모로 분리하고 각 자모에 대한 타이밍 정보를 제공합니다. 이것은 복잡하며 정확하려면 오디오 분석이 필요합니다.
IINA 통합: 주요 사용 사례는 IINA 미디어 플레이어에서 작동하는 수정된 SMI 파일을 생성하여 비디오 재생 중에 가라오케 효과를 활성화하는 것입니다.
3. 제한 사항

SMI 형식 제한: SMI 형식 자체에는 다음과 같은 제한이 있습니다.
단일 자막 블록 내에서 단어의 개별 부분에 스타일을 지정하는 기능(예: 글자 절반의 색상 변경)을 기본적으로 지원하지 않습니다. 이로 인해 진정한 하위 음절(자모 수준) 가라오케가 어렵습니다.
중첩된 <font> 태그(음절 내에서 색상을 변경하는 데 사용됨)는 모든 SMI 플레이어에서 지원된다는 보장이 없습니다.
타이밍 정확도: 가라오케 효과의 정확도는 원본 SMI 파일에 제공된 타이밍 정보에 전적으로 의존합니다. SMI의 타이밍이 맞지 않으면 가라오케 효과도 맞지 않습니다.
내장 오디오 분석 없음: 이 프로그램은 개별 음절 또는 자모의 지속 시간을 결정하기 위한 오디오 분석을 수행하지 않습니다. SMI 파일에 제공된 타이밍에 전적으로 의존합니다. 정확한 자모 수준 가라오케를 위해서는 외부 오디오 분석이 필요합니다.
글꼴 지원: 자막이 올바르게 표시되려면 사용자는 시스템에 적절한 글꼴을 설치해야 합니다. 이는 라틴 문자가 아닌 문자 집합(한국어, 일본어, 중국어 등)에 특히 중요합니다. 프로그램 자체는 글꼴을 설치하지 않습니다. Noto Sans CJK JP를 적극 권장합니다.
IINA 관련 특이 사항: IINA용으로 설계되었지만 자막 렌더링에 영향을 미치는 IINA 관련 동작이나 제한 사항이 있을 수 있습니다.
4. 사용법

종속성 설치:

Python 3.6 이상.
jamo 라이브러리 (선택 사항, 자모 수준 가라오케용): pip install jamo
프로그램 실행:

Bash
python your_script_name.py input.smi [output.smi]
your_script_name.py: Python 스크립트의 이름.
input.smi: 입력 SMI 파일의 경로.
output.smi (선택 사항): 출력 SMI 파일의 경로. 생략하면 출력이 콘솔에 인쇄됩니다.
IINA 구성:

IINA로 비디오 파일을 엽니다.
자막 -> 기본 설정... (또는 설정...)으로 이동합니다.
"텍스트 자막"에서 "글꼴"을 Noto Sans CJK JP(또는 해당 언어를 지원하는 다른 글꼴)로 설정합니다. 올바른 표시를 위해서는 반드시 필요합니다.
"글꼴 크기", "글꼴 색상", "외곽선" 및 "그림자"를 원하는 대로 조정합니다.
IINA의 "자막 인코딩" 설정이 SMI 파일의 실제 인코딩과 일치하는지 확인합니다.
수정된 자막 로드:

출력을 새 SMI 파일(output.smi)에 저장한 경우 IINA에서 이 파일을 로드합니다.
가라오케 효과가 적용된 자막이 표시됩니다.
5. 중요 참고 사항:

글꼴 설치: 문자의 올바른 표시는 운영 체제에 올바른 글꼴이 설치되어 있는지 여부에 따라 크게 달라집니다. 한국어, 일본어 또는 중국어 자막의 경우 Noto Sans CJK JP를 적극 권장합니다.
인코딩: 깨진 문자가 표시되면 SMI 파일의 인코딩을 다시 확인하고 Python 스크립트와 IINA의 인코딩 설정과 일치하는지 확인하세요.
SMI 호환성: 프로그램은 유효한 SMI 파일을 생성하려고 하지만 다른 플레이어와의 호환성 문제가 있을 가능성은 항상 있습니다.
자모 수준 가라오케: 제공된 자모 수준 가라오케 구현은 주로 개념적입니다. 정확하게 작동하려면 일반적으로 오디오 분석에서 얻은 각 자모에 대한 매우 정확한 타이밍 정보를 제공해야 합니다.
6. 코드 설명 (주요 부분):

detect_encoding(file_path): SMI 파일의 인코딩을 감지합니다.
parse_smi_content(smi_file, encoding): SMI 파일을 파싱하여 타이밍과 텍스트를 추출합니다. HTML 엔터티 및 <br> 태그를 처리합니다.
apply_karaoke_effect_syllable(smi_blocks, current_time): current_time을 기반으로 <font> 태그를 추가하여 음절 수준 가라오케 효과를 적용합니다.
main(): 파일 입/출력을 처리하고 파싱 및 가라오케 효과 함수를 호출합니다.



---

1. Introduction

This Python program converts SMI (Synchronized Multimedia Integration Language) subtitle files into a format suitable for karaoke-style subtitle display. It allows for dynamic color changes in the subtitles, synchronized with the audio, to highlight the currently spoken syllable (or, optionally, individual phonemes). The primary target is to enable a basic karaoke effect in video players like IINA that support SMI subtitles, but may have limitations.

2. Functionality

SMI Parsing: The program parses SMI files, extracting the timing information (<SYNC> tags) and text content of each subtitle block.
Encoding Detection: It attempts to automatically detect the encoding of the SMI file (UTF-8, EUC-KR, Shift-JIS, etc.) to handle various character sets correctly.
HTML Entity Handling: It correctly handles HTML entities like &nbsp; (non-breaking space), &lt; (less than), &gt; (greater than), and &amp; (ampersand).
Karaoke Effect (Syllable-based):
The core feature is applying a karaoke effect by changing the color of each syllable as it's being spoken.
The color changes are based on the Start time specified in the <SYNC> tags in the SMI file.
The program uses <font color="..."> tags to achieve this effect, wrapping each syllable with the appropriate color tag.
Karaoke Effect (Jamo-based Conceptual):
The program can conceptually support finer-grained karaoke effects at the level of individual Jamo (Korean alphabet's basic components).
This would use the jamo library to separate syllables into their constituent Jamo, providing timing information for each. This is complex and requires audio analysis to be accurate.
IINA Integration: The primary use case is to create modified SMI files that work with the IINA media player, enabling a karaoke effect during video playback.
3. Limitations

SMI Format Restrictions: The SMI format itself has limitations:
It doesn't natively support styling individual parts of a word within a single subtitle block (e.g., changing the color of half a letter). This makes true sub-syllabic (jamo-level) karaoke difficult.
Nested <font> tags (used to change color within a syllable) are not guaranteed to be supported by all SMI players.
Timing Accuracy: The karaoke effect's accuracy is entirely dependent on the timing information provided in the original SMI file. If the SMI's timing is off, the karaoke effect will be off as well.
No Built-in Audio Analysis: The program doesn't perform any audio analysis to determine the duration of individual syllables or jamo. It relies entirely on the timing provided in the SMI file. For accurate jamo-level karaoke, external audio analysis would be necessary.
Font Support: The user must have appropriate fonts installed on their system for the subtitles to display correctly. This is especially important for non-Latin character sets (Korean, Japanese, Chinese, etc.). The program itself doesn't install fonts. Noto Sans CJK JP is strongly recommended.
IINA-Specific Quirks: While designed for IINA, there might be IINA-specific behaviors or limitations that affect subtitle rendering.
4. Usage

Install Dependencies:

Python 3.6 or later.
jamo library (optional, for jamo-level karaoke): pip install jamo
Run the Program:

Bash
python your_script_name.py input.smi [output.smi]
your_script_name.py: The name of your Python script.
input.smi: The path to the input SMI file.
output.smi (optional): The path to the output SMI file. If omitted, the output will be printed to the console.
Configure IINA:

Open the video file with IINA.
Go to Subtitles -> Preferences... (or Settings...).
Under "Text Subtitles," set the "Font" to Noto Sans CJK JP (or another font that supports your language). This is critical for correct display.
Adjust "Font size," "Font color," "Outline," and "Shadow" as desired.
Ensure that the "Subtitle Encoding" setting in IINA matches the actual encoding of your SMI file.
Load the Modified Subtitle:

If you saved the output to a new SMI file (output.smi), load this file in IINA.
You should see the subtitles with the karaoke effect.
5. Important Notes:

Font Installation: The correct display of characters depends heavily on having the right fonts installed on your operating system. For Korean, Japanese, or Chinese subtitles, Noto Sans CJK JP is highly recommended.
Encoding: If you see garbled characters, double-check the encoding of the SMI file and make sure it matches the encoding settings in both the Python script and IINA.
SMI Compatibility: While the program tries to create valid SMI files, there's always a possibility of compatibility issues with different players.
Jamo-level Karaoke: The jamo-level karaoke implementation provided is mostly conceptual. For it to work accurately, you would need to provide very precise timing information for each jamo, typically obtained from audio analysis.
6. Code Explanation (Key Parts):

detect_encoding(file_path): Detects the encoding of the SMI file.
parse_smi_content(smi_file, encoding): Parses the SMI file, extracting timing and text. Handles HTML entities and <br> tags.
apply_karaoke_effect_syllable(smi_blocks, current_time): Applies the syllable-level karaoke effect by adding <font> tags based on the current_time.
main(): Handles file input/output, calls the parsing and karaoke effect functions.
---

### Конвертер SMI в ASS

#### Обзор
Конвертер SMI в ASS — это приложение с графическим интерфейсом, преобразующее субтитры формата SMI в формат ASS. Пользователь может настраивать шрифты, размер шрифта и длительность субтитров, а также поддерживается пакетное преобразование нескольких файлов.

#### Основные функции
- Преобразование файлов SMI в формат ASS
- Настраиваемые шрифты и размеры шрифтов
- Поддержка пакетного преобразования
- Функция ведения журнала ошибок

#### Как использовать
1. Запустите программу
2. Выберите файл SMI или папку для преобразования
3. Укажите выходную папку
4. Настройте шрифт и другие параметры
5. Нажмите кнопку "Начать преобразование"

---

### SMI-zu-ASS-Konverter-Handbuch

#### Übersicht
Der SMI-zu-ASS-Konverter ist eine GUI-basierte Anwendung zur Umwandlung von SMI-Untertiteldateien in das ASS-Format. Benutzer können Schriftarten, Schriftgrößen und Untertiteldauer anpassen. Außerdem wird die Stapelverarbeitung mehrerer Dateien unterstützt.

#### Hauptfunktionen
- Konvertiert SMI-Dateien in das ASS-Format
- Einstellbare Schriftarten und Schriftgrößen
- Unterstützung für die Stapelverarbeitung
- Fehlerprotokollierungsfunktion

#### Verwendung
1. Starten Sie das Programm
2. Wählen Sie die SMI-Datei oder den Ordner aus
3. Legen Sie den Ausgabeordner fest
4. Passen Sie die Schriftart und weitere Einstellungen an
5. Klicken Sie auf die Schaltfläche "Konvertierung starten"

---

### SMIからASSへのコンバーター マニュアル

#### 概要
SMIからASSへのコンバーターは、SMI字幕ファイルをASS形式に変換するGUIベースのアプリケーションです。フォント、フォントサイズ、字幕の表示時間を設定でき、複数ファイルの一括変換にも対応しています。

#### 主な機能
- SMIファイルをASS形式に変換
- フォントとフォントサイズの調整可能
- 複数ファイルの一括変換対応
- エラーログ記録機能

#### 使い方
1. プログラムを起動
2. 変換するSMIファイルまたはフォルダーを選択
3. 出力フォルダーを指定
4. フォントやその他の設定を調整
5. "変換開始" ボタンをクリック

---

### Manuel du Convertisseur SMI en ASS

#### Aperçu
Le convertisseur SMI en ASS est une application basée sur une interface graphique permettant de convertir des fichiers de sous-titres SMI au format ASS. Les utilisateurs peuvent configurer les polices, la taille de police et la durée des sous-titres. Il prend également en charge la conversion par lots de plusieurs fichiers.

#### Fonctionnalités principales
- Conversion des fichiers SMI en format ASS
- Personnalisation des polices et tailles de police
- Prise en charge de la conversion par lots
- Fonctionnalité de journalisation des erreurs

#### Mode d’emploi
1. Exécutez le programme
2. Sélectionnez le fichier SMI ou le dossier à convertir
3. Spécifiez le dossier de sortie
4. Ajustez la police et d’autres paramètres
5. Cliquez sur le bouton "Démarrer la conversion"
