![GitHub repo size](https://img.shields.io/github/repo-size/hunar4321/life_code)
![GitHub](https://img.shields.io/github/license/hunar4321/life_code)

# Particle Life シミュレーション
原子のような粒子間の引力・斥力という単純なルールを用いて、原始的な人工生命をシミュレーションするシンプルなプログラムです。複雑で自己組織化する生命のようなパターンが生成されます。GUI要素を除けば、コードは1ページ未満です。動画チュートリアルとウォークスルーは以下からご覧いただけます。

詳しくはこちら（YouTube動画チュートリアル）:
-----------------------------------------------
https://youtu.be/0Kx4Y9TVMGg

オンラインデモ（JavaScript版）:
-------------
ライブデモはこちら（JavaScript）:
  - 2D - https://hunar4321.github.io/particle-life/particle_life.html
  - 3D - https://hunar4321.github.io/particle-life/particle_life_3d.html
  
インターフェース（C++版）
--------------------------------------------------------
![](images/interface.jpg)

実行結果の例
--------------------------------------------------------
![](images/big_pic.jpg)

再現できる興味深いパターン:
-------------------------------------
これらのパターンを再現するために、パラメータを正確に合わせる必要はありません。興味深いパターンを得る最良の方法は、まずランダムにパラメータを探索し、面白いパターンを見つけたら徐々に微調整することです。局所最適に陥らないようにするため、時折パラメータを大きく変化させてください。こうすることで、興味深く多様なパターンが次々と現れます。

![](images/some_patterns.jpg)

使い方:
-------------
このリポジトリをダウンロードし、ファイルを解凍してから /particle_life/bin/ フォルダに移動し、particle_life.exe をクリックしてください。

コード:
----------------
ソースコードはC++、JavaScript、Pythonで利用可能です。
ウォークスルーチュートリアルはこちらのYouTube動画をご覧ください: https://youtu.be/0Kx4Y9TVMGg

C++プログラムに貢献したい場合、コアアルゴリズムは "/particle_life/src/ofApp.cpp" の最初の100行にあります。残りはGUIコンポーネントとレンダリング制御で、オープンソースで使いやすい画像レンダリングライブラリであるopenFrameworksライブラリによって提供されています。

始めるには、このリポジトリをダウンロードしてから、こちらからopenFrameworksライブラリをダウンロードしてください: https://openframeworks.cc/。openFrameworkのprojectGeneratorを使用して、/particle_life/ フォルダをプロジェクトにインポートします。

あるいは、新しいopenFrameworkプロジェクトを生成してofxGuiを追加してください。プロジェクトファイルが生成されたら、/src/ フォルダをここで提供されているものに置き換えてください。

これでお使いのマシンでC++コードをコンパイルできます。

JavaScriptのコードはこれだけでシンプルです:
-------------------------------------
より最適化されたバージョンについては particle_life.html ファイルもご覧ください。貢献してくださった皆さんに感謝します。

```html
<canvas id="life" width="500" height="500"></canvas>
<script>
  //Hunar Ahmad @ brainxyz
  m = document.getElementById("life").getContext("2d");
  draw = (x, y, c, s) => {
    m.fillStyle = c;
    m.fillRect(x, y, s, s);
  };
  atoms = [];
  atom = (x, y, c) => {
    return { x: x, y: y, vx: 0, vy: 0, color: c };
  };
  random = () => {
    return Math.random() * 400 + 50;
  };
  create = (number, color) => {
    group = [];
    for (let i = 0; i < number; i++) {
      group.push(atom(random(), random(), color));
      atoms.push(group[i]);
    }
    return group;
  };
  rule = (atoms1, atoms2, g) => {
    for (let i = 0; i < atoms1.length; i++) {
      fx = 0;
      fy = 0;
      for (let j = 0; j < atoms2.length; j++) {
        a = atoms1[i];
        b = atoms2[j];
        dx = a.x - b.x;
        dy = a.y - b.y;
        d = Math.sqrt(dx * dx + dy * dy);
        if (d > 0 && d < 80) {
          F = (g * 1) / d;
          fx += F * dx;
          fy += F * dy;
        }
      }
      a.vx = (a.vx + fx) * 0.5;
      a.vy = (a.vy + fy) * 0.5;
      a.x += a.vx;
      a.y += a.vy;
      if (a.x <= 0 || a.x >= 500) { a.vx *= -1; }
      if (a.y <= 0 || a.y >= 500) { a.vy *= -1; }
    }
  };
  yellow = create(200, "yellow");
  red = create(200, "red");
  green = create(200, "green");
  update = () => {
    rule(green, green, -0.32);
    rule(green, red, -0.17);
    rule(green, yellow, 0.34);
    rule(red, red, -0.1);
    rule(red, green, -0.34);
    rule(yellow, yellow, 0.15);
    rule(yellow, green, -0.2);
    m.clearRect(0, 0, 500, 500);
    draw(0, 0, "black", 500);
    for (i = 0; i < atoms.length; i++) {
      draw(atoms[i].x, atoms[i].y, atoms[i].color, 5);
    }
    requestAnimationFrame(update);
  };
  update();
</script>
```
</br>

その他の移植版:
-----------------------
- [Godot](https://github.com/NiclasEriksen/game-of-leif)
- [Rust](https://github.com/ChevyRay/smarticles)
- [Go-1](https://github.com/sikora507/go-artificial-life), [Go-2](https://github.com/fglo/particles-rules-of-attraction), [Go-3](https://github.com/youssefboulmalf/Particle-Life-Go)
- [Python](https://github.com/gianfa/pyrticleslife)
- [Lua](https://github.com/ravener/love-life)
- [QB64-PE](https://github.com/a740g/Particle-Life)
- [Webgl](https://github.com/CapsAdmin/webgl-particles)
- [Java](https://github.com/helloimalemur/ParticleSimulation)
- [C# Winforms](https://github.com/BlinkSun/ParticleLifeSimulation)
- [FreeBasic](https://www.freebasic.net/forum/viewtopic.php?p=294331#p294331)
- [Julia](https://github.com/RomeoV/ParticleLife.jl)
- [Flutter](https://github.com/JillyTaboga/particles)


関連トピック:
--------------------
Particle Lifeシミュレーション、
原始スープ - 進化、
コンウェイのライフゲーム、
セルオートマトン、
自己組織化パターン、

このプロジェクトは Jeffery Ventrella の Clusters に触発されました: http://www.ventrella.com/Clusters/。Ventrellaのコードにはアクセスできませんが、このプロジェクトと他のParticle Lifeプロジェクトとの主な違いは、衝突判定を実装しなかったことであり、これにより数千個の粒子をリアルタイムでシミュレーションすることが可能になりました。また、パラメータをリアルタイムで変更できるGUIコントロールを追加することで、簡単な微調整と探索が可能になり、非常にシンプルな関係モデルから、これまで見たことのないパターンが出現するのを発見することができました。
ここのコードは、おそらく他のどの人工生命コードよりも桁違いにシンプルです。なぜなら、このコードは非プログラマーや一般の人々向けの教育資料として、「複雑さはシンプルさから生まれる」ことを証明するためだけに始めたものだからです。


やることリスト:
--------------------
1. パラメータの保存と読み込み機能の追加（見つけた興味深いモデルを簡単に共有できるようにするため）
2. 粒子タイプを追加できる機能（現在は4種類の粒子タイプに固定されています）
3. 現在、最大のボトルネックはネストされたforループ（全粒子間のペアワイズ距離を計算する）であり、計算量が二次関数的になっています。これを回避する方法が見つかれば素晴らしいです。
4. 3の代替案として、ペアワイズ距離の計算は恥ずかしいほど並列化可能なので、GPUで計算できます。
5. 画面のリサイズ機能の追加と境界チェックの改善（高速移動する粒子が画面外に飛び出す場合があるため）
6. パラメータをより細かく制御できる、より直感的なUIの追加。
7. ランダム化ボタンの追加、あるいはさらに良いのは、初期ルールを連続的かつ再帰的に変異させるシンプルなメタルールを持つことです。これにより、パターンが局所最適に陥ることなく、常に変化し続けます!
8. より良い微調整方法は、進化アルゴリズムを使用してパラメータを選択・最適化することですが、そのためには適応度関数を書く必要があります。このプログラムの領域で適応度関数が何に相当するのか、現時点ではわかりません。私たちの世界では適応度関数は競争と適者生存です。しかし、ここでは私たちにとって興味深いパターンを生成するパラメータを微調整・選択しますが、「興味深い」という言葉は言うのは簡単でも定義するのは難しいのです!

