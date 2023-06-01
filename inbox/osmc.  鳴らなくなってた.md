---
type: note
---

#raspberry_pi/osmc 

---
2022-06-02  20:02

# osmc.  鳴らなくなってた

ある日音楽を聞こうとしたら全然音が鳴らなくなってた。

Program add-ons --> My OSMC -> Hardware Support --> Soundcard Overlay  

![[IMG_0737のコピー.jpg]]

ここから My OSMC をクリックする。

![[IMG_0738のコピー.jpg]]

ここから Hardware Support --> Soundcard Overlay で

allo-boss-dac-pcm512x-audio にして再起動したら音が鳴った。

ここが none になってたので、アップデートがかかってとかして?初期状態になってたのが原因。

