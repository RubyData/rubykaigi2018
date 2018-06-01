# RubyData Sendai Workshop in RubyKaigi 2018

Run the following commands.

```
git clone https://github.com/RubyData/rubykaigi2018.git
cd rubykaigi2018
rake docker:pull
rake docker:run:notebook volume=.:/home/jovyan/rubykaigi2018
```

Then open <http://localhost:8888/?token=rubykaigi2018>.
