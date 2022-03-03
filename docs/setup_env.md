# Setup środowiska dla nowego projektu

Uwaga: Jeśli korzystamy z [JupyterHub](notebooks.md), wszystkie poniższe komendy należy odpalać z prefiksiem `sudo -E env PATH=${PATH}`.

0. Jeśli czujemy się odważni, to można zrobić update condy:

```
conda update -n base conda
```

1. Tworzymy nowe środowisko. Instalujemy paczkę `ipykernel`, jeśli korzystamy z JupyterHub, por. [notebooks](notebooks.md).

```
conda create --name dl_template python=3.8 pip ipykernel
```

2. Aktywujemy środowisko:

```
conda activate dl_template
```

3. Instalujemy Pytorch oraz cudatoolkit. Ten punkt niestety zależy od maszyny: sprawdzamy wersję driverów GPU (jeśli w ogóle mamy) poleceniem `nvidia-smi` (np. na kulfonie `470.63.01`), i wybieramy najnowszą wersję cudatoolkit kompatybilną z tymi driverami wg [tej tabelki](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html) (np. 11.1). Jednocześnie condą instalujemy pytorcha, żeby dostać kompatybilny build. Na [stronie](https://pytorch.org/get-started/previous-versions/) znajdujemy wersję torcha kompatybilną z wersją CUDA. Rekomendujemy dodać numer konkretnej wersji oraz tego by pakiet pytorch pochodził z kanału pytorch, jak poniżej (inaczej wszelkie dalsze zmiany w condzie mogą nam niechcący podmienić na niższą/wyższą wersję lub wersję cpu zamiast cuda).

Przykładowa instalacja na szklance (starsze sterowniki)

```
conda install cudatoolkit=10.1 "pytorch::pytorch==1.7.1" "torchvision==0.8.2" -c pytorch -c conda-forge
```

Instalacja na kulfonie:

```
conda install pytorch torchvision torchaudio cudatoolkit=11.1 -c pytorch -c nvidia
```


3. *Konfiguracja na gcloud/dockerfile sprowadza się do tego kroku (zakładając, że w obrazach mamy już Pytorcha i cudatoolkit)*. Z uwagi na kwadratowy algorytm rozwiązywania zależności przez condę, jak również rozmaite problemy cross-platformowe, staramy się instalować pip-em. Instalujemy tylko główne paczki. W tym celu odpalamy z poziomu root-a projektu:

```
bash env_setup.sh
```

5. Jeśli doinstalowujemy nową paczkę i będziemy z niej korzystać w projekcie (albo zauważyliśmy, że jakiejś brakuje), powinniśmy ją dodać w odpowiednim miejscu w pliku `./env_setup.sh`.