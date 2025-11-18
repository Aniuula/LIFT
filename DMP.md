# Data Managment Plan

## Zakres i typ danych

### **Vimeo-90K — główny zbiór treningowy**

**Typy plików:**  
- klipy wideo (89 800 klipów)  
- generowane sekwencje:  
  - **triplet** (3-klatkowe)  
  - **septuplet** (7-klatkowe)

**Rozmiary:**  
- **Triplet:** 73 171 sekwencji 3-klatkowych, rozdzielczość **448×256**  
  - łączny rozmiar (train + test): **~33 GB**
- **Septuplet:** 91 701 sekwencji 7-klatkowych, rozdzielczość **448×256**  
  - łączny rozmiar (train + test): **~82 GB**

**Pochodzenie:**  
👉 http://toflow.csail.mit.edu/

---

### **X4K1000FPS — testy ekstremalnych ruchów**

Wykorzystany do oceny wydajności modelu przy:  
- bardzo szybkich ruchach,  
- długim kontekście czasowym (lepsze porównanie z RIFE).

**Typy plików:**  
- sekwencje klatek jako obrazy **PNG**

**Rozdzielczości:**  
- **X-TEST:** 4096×2160 (4K)  
- **X-TRAIN:** 768×768 (crop z 4K)

**Pochodzenie:**  
👉 https://www.dropbox.com/sh/duisote638etlv2/AABJw5Vygk94AWjGM4Se0Goza?dl=0

---

### **UCF101 — testy na realnych nagraniach (YouTube-like)**


**Typy plików:**  
- klipy wideo podzielone na **101 klas akcji**

**Rozmiary:**  
- **13 320 klipów**  
- łącznie około **27 godzin wideo**

**Pochodzenie:**  
👉 https://www.crcv.ucf.edu/data/UCF101.php
## Prawa i licencje
### **Vimeo-90K**

**Autorzy:** Tianfan Xue, Baian Chen, Jiajun Wu, Donglai Wei, William T. Freeman  
**Źródło:** „Video Enhancement with Task-Oriented Flow (TOFlow)”, ECCV 2018  
**Licencja / prawa:**
- Zbiór dostępny wyłącznie do celów **akademickich i badawczych**  
---

### **X4K1000FPS**

**Autorzy:** Jongyoo Kim, Jihye Back, Suhwan Cho, Jongwon Choi  
**Źródło:** „X4K1000FPS: High-Speed, High-Resolution Dataset for Video Frame Interpolation”  
**Licencja / prawa:**
- Dostępny wyłącznie do **celów badawczych**  

---

### **UCF101**

**Autorzy:** Khurram Soomro, Amir Roshan Zamir, Mubarak Shah  
**Źródło:** „UCF101: A Dataset of 101 Human Actions Classes From Videos in The Wild”  
**Licencja / prawa:**
- Dozwolone użycie **wyłącznie do badań i edukacji**  
- Materiały wideo należą do oryginalnych właścicieli (wiele z YouTube)  

## Formaty i standardy


- **Vimeo-90K** – klatki przechowywane jako pojedyncze obrazy **PNG (lossless)**  
- **X4K1000FPS** – sekwencje udostępnione jako obrazy **PNG (lossless)**  
- **UCF101** – materiały wideo zapisane w formacie **AVI**

## Metadane


- **encoder.py** – konfiguracja i metadane ekstraktora cech  
  *(Stage 1: Feature Extraction)*

- **transformer.py** – definicja warstw transformera oraz mechanizmu agregacji czasowej  
  *(Stage 2: Windowed Temporal Attention)*

- **ifnet.py** – implementacja kaskadowego modelu estymacji przepływu optycznego  
  *(Stage 3: Multi-scale Flow Estimation)*

- **synthesis.py** – logika syntezy wstępnej klatki na bazie przewidywanego przepływu  
  *(Stage 4: Coarse Synthesis)*

- **refine.py** – moduł dopracowania obrazu w pełnej rozdzielczości  
  *(Stage 5: Full-resolution Refinement)*

- **warplayer.py** – funkcje i parametry operacji warpowania (warp, backward warp, flow sampling)

- **loss.py** – zestaw funkcji kosztu (rekonstrukcja, perceptual, smoothness)

- **lift.py** – główny wrapper architektury LIFT, integrujący wszystkie etapy pipeline’u  
  *(Model end-to-end)*

## Przechowywanie i bezpieczeństwo

Projekt korzysta z trzech lokalizacji danych:

### **Kod źródłowy**
- Przechowywany w publicznym repozytorium GitHub  
- Repo zawiera: kod, konfiguracje, dokumentację  
 

### **Dane treningowe**
- Przechowywane lokalnie lub na zewnętrznych dyskach 
- Zbiory: **Vimeo90K, UCF101, X4K1000FPS**  

### **Wagi modeli**
- Zapisywane lokalnie
  
## Wersjonowanie i reproducibility
- Kod wersjonowany przez GitHub (commit history)  
- Konfiguracje przechowywane w `configs/`

  Projekt zapewnia reproducibility dzięki:
- stałej strukturze repozytorium  
- zapamiętanym hiperparametrom w `configs/`    
  
## Udostępnianine i cytowanie

Repozytorium jest **tymczasowo prywatne**, dopóki projekt nie zostanie ukończony i zweryfikowany.  
Po zakończeniu prac planowane jest udostępnienie go w formie publicznej wraz z dokumentacją.

Zbiory danych nie mogą być udostępniane bezpośrednio ze względu na ograniczenia licencyjne — projekt zawiera jedynie linki i instrukcje ich pobierania.

### **Cytowanie**

Po upublicznieniu repozytorium zalecany format cytowania:

```bibtex
@misc{LIFT2025,
  author        = {A. <Celińska>, J. <Kubiński>, P. <Niewiński>},
  title         = {LIFT: Long-range Interpolation with Far Temporal Context},
  year          = {2025},
  howpublished  = {\url{https://github.com/<Aniuula>/LIFT}}
