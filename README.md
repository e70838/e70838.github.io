# e70838.github.io
This is not a blog. I am just learning how to create a website hosted on github.

Ceci n'est pas un blog. J'apprends juste à créer un site web hébergé sur github.

Comme indiqué sur [pages.github.com](https://pages.github.com) il faut créer un repository nommé *username*.github.io (pas seulement username).

Dans les settings du projet, (section GitHub Pages), on peut choisir d'utiliser soit une branche gh-pages, soit le répertoire principal de la branche master, soit le répertoire docs de la branche master.

Sur la page [help.github.com/en/github/working-with-github-pages/about-github-pages-and-jekyll](https://help.github.com/en/github/working-with-github-pages/about-github-pages-and-jekyll), il est expliqué que certains plugin sont activés par défaut, comme:

[github.com/benbalter/jekyll-readme-index](https://github.com/benbalter/jekyll-readme-index)

C'est ce plugin qui permet de se passer de index.md

L'index des posts est dans [e70838.github.io/posts.html](https://e70838.github.io/posts.html)

Step 2:
Pour pouvoir faire des expériences sans avoir à faire de commit, j'ai [installé Jekyll localement](https://jekyllrb.com/docs/installation/ubuntu/).

Ensuite,
  cd ~/Documents/Jef/github/e70838.github.io
et
  bundle exec jekyll serve

Bang, j'ai l'erreur
  Could not locate Gemfile or .bundle/ directory
telle que décrite sur [github.com/jekyll/jekyll/issues/5719](https://github.com/jekyll/jekyll/issues/5719)

La solution proposée qui consiste à faire `apt-get install ruby-all-dev` et de refaire `gem install jekyll bundler` était prometeuse, mais n'a eu aucun effet.

J'ai essayé
  jekyll serve
puis
  jekyll serve --trace

ce qui a donné: The jekyll-theme-hacker theme could not be found.

J'ai essayé
  gem install github-pages

puis relancé jekyll serve --trace

C'est pas top, mais suffisant pour le plus gros.
Il semble qu'il manque surtout la section plugins de _config.yml. 

Il y a 2 plugins que ne veulent pas fonctionner: jekyll-default-layout et jekyll-github-metadata

Tant pis, j'enlève.