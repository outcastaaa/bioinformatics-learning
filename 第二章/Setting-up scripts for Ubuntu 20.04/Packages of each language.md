
# Packages of each language

```shell script
bash $HOME/Scripts/dotfiles/perl/install.sh

bash $HOME/Scripts/dotfiles/python/install.sh

bash $HOME/Scripts/dotfiles/r/install.sh

# Optional
# bash $HOME/Scripts/dotfiles/rust/install.sh

```

# perl
## bash $HOME/Scripts/dotfiles/perl/install.sh中内容：
```
#!/bin/bash

BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )

cd "${BASE_DIR}" || exit

#将目录挂载到$HOME/Scripts/dotfiles/perl/的目录，pwd显示当前目录，将结果赋值给BASE_DIR，然后退出
#上面两步是为了将挂载目录，将下列安装 安装在$HOME/Scripts/dotfiles/perl/


hash cpanm 2>/dev/null || {
    curl -L https://cpanmin.us | perl - App::cpanminus
}
#先在hash中寻找cpamn，如果执行失败则在网页https://cpanmin.us中下载，将参数代入perl - App::cpanminus


CPAN_MIRROR=https://mirrors.ustc.edu.cn/CPAN/
NO_TEST=--notest
#设定CPAN-MIRROR 参数， 为这个镜像源


# basic modules   完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Archive::Extract Config::Tiny File::Find::Rule Getopt::Long::Descriptive JSON JSON::XS Text::CSV_XS YAML::Syck
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST App::Ack App::Cmd DBI MCE Moo Moose Perl::Tidy Template WWW::Mechanize XML::Parser
#为了加快 cpanm 下载速度, 可以指定使用镜像. 并只从镜像下载
#$CPAN_MIRROR为制定镜像源



# RepeatMasker need this  完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Text::Soundex

# GD   完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST GD SVG GD::SVG

# bioperl  完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Data::Stag Test::Most URI::Escape Algorithm::Munkres Array::Compare Clone Error File::Sort Graph List::MoreUtils Set::Scalar Sort::Naturally
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST HTML::Entities HTML::HeadParser HTML::TableExtract HTTP::Request::Common LWP::UserAgent PostScript::TextBlock
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST XML::DOM XML::DOM::XPath XML::SAX::Writer XML::Simple XML::Twig XML::Writer GraphViz SVG::Graph
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST SHLOMIF/XML-LibXML-2.0134.tar.gz
cpanm --mirror-only --mirror $CPAN_MIRROR --notest Convert::Binary::C IO::Scalar
cpanm --mirror-only --mirror $CPAN_MIRROR --notest CJFIELDS/BioPerl-1.007002.tar.gz

cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Bio::ASN1::EntrezGene Bio::DB::EUtilities Bio::Graphics
cpanm --mirror-only --mirror $CPAN_MIRROR --notest CJFIELDS/BioPerl-Run-1.007002.tar.gz # BioPerl-Run

# circos 完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Config::General Data::Dumper Digest::MD5 Font::TTF::Font Math::Bezier Math::BigFloat Math::Round Math::VecStat
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Params::Validate Readonly Regexp::Common Set::IntSpan Statistics::Basic Text::Balanced Text::Format Time::HiRes

# Bio::Phylo 完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST XML::XML2JSON PDF::API2 Math::CDF Math::Random
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Bio::Phylo

# Database and WWW  完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST MongoDB LWP::Protocol::https Mojolicious

# text, rtf and xlsx 完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Roman Text::Table RTF::Writer Chart::Math::Axis
cpanm --mirror-only --mirror $CPAN_MIRROR --notest Excel::Writer::XLSX Spreadsheet::XLSX Spreadsheet::ParseExcel Spreadsheet::WriteExcel

# Test::* 完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Test::Class Test::Roo Test::Taint Test::Without::Module

# Moose and Moo 完成
cpanm --mirror-only --mirror $CPAN_MIRROR --notest MooX::Options MooseX::Storage

# Develop 完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST App::pmuninstall App::cpanoutdated Minilla Version::Next CPAN::Uploader

# Others  完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST DateTime::Format::Natural DBD::CSV String::Compare Sereal PerlIO::gzip

# AlignDB::*  完成
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST AlignDB::IntSpan AlignDB::Stopwatch
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST AlignDB::Codon AlignDB::DeltaG AlignDB::GC AlignDB::SQL AlignDB::Window AlignDB::ToXLSX
cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST App::RL App::Fasops App::Rangeops

# App::* 完成
cpanm -nq https://github.com/wang-q/App-Plotr.git
cpanm -nq https://github.com/wang-q/App-Egaz.git
#cpanm -n --notest 跳过测试模块节省时间，-q --quiet 只把成功或者失败的依赖包显示到标准输出



# Gtk3 stuffs
# cpanm --mirror-only --mirror $CPAN_MIRROR $NO_TEST Glib Cairo Cairo::GObject Glib::Object::Introspection Gtk3 Pango

# Math
# cpanm --mirror-only --mirror $CPAN_MIRROR --notest Math::Random::MT::Auto PDL Math::GSL

# Statistics::R would be installed in `brew.sh`
# DBD::mysql would be installed in `mysql8.sh`
```





# python

## bash $HOME/Scripts/dotfiles/python/install.sh内容：
```
#!/bin/bash

BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )

cd "${BASE_DIR}" || exit
#将目录挂载到$HOME/Scripts/dotfiles/Python/的目录，如果不成功则退出
#上面两步是为了将挂载目录，将下列安装 安装在$HOME/Scripts/dotfiles/Python/



# pip
PYPI_MIRROR=https://pypi.tuna.tsinghua.edu.cn/simple

#pip3 install -i ${PYPI_MIRROR} --upgrade pip setuptools
#pip3 install默认调用的是国外地址库，下载过程会因为timeout问题导致失败。
pip3 install -i 镜像simple网址 --trusted-host 镜像域名 所需要安装的库名，国内比较常用的镜像


pip3 install -i ${PYPI_MIRROR} pysocks cryptography
pip3 install -i ${PYPI_MIRROR} more-itertools zipp setuptools-scm
pip3 install -i ${PYPI_MIRROR} numpy matplotlib
pip3 install -i ${PYPI_MIRROR} tqdm tables networkx dataclasses
pip3 install -i ${PYPI_MIRROR} plotly gmpy2 colorlover bokeh
pip3 install -i ${PYPI_MIRROR} pandas sympy
pip3 install -i ${PYPI_MIRROR} jupyter scipy
pip3 install -i ${PYPI_MIRROR} lxml statsmodels patsy
pip3 install -i ${PYPI_MIRROR} beautifulsoup4 scikit-learn seaborn

pip3 install -i ${PYPI_MIRROR} Circle-Map cutadapt importlib-metadata

# poetry can search packages
# curl -sSL https://install.python-poetry.org | python3 -
```







# R
## bash $HOME/Scripts/dotfiles/r/install.sh内容：
```
#!/bin/bash

BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )

hash Rscript 2>/dev/null || {
    brew install r
}
#安装R


# hash gcc-5 2>/dev/null || {
#     brew install gcc@5
# }

# hash udunits2 2>/dev/null || {
#     brew install udunits
# }

cd "${BASE_DIR}" || exit
#将目录挂载到$HOME/Scripts/dotfiles/r/的目录，如果不成功则退出
#上面两步是为了将挂载目录，将下列安装 安装在$HOME/Scripts/dotfiles/r/




# font_install() doesn't provide the repo argument
cat <<EOF > .Rprofile
# set R mirror
# Bioconductor mirror
options(BioC_mirror="https://mirrors4.tuna.tsinghua.edu.cn/bioconductor")
# CRAN mirror
options("repos" = c(CRAN="https://mirrors4.tuna.tsinghua.edu.cn/CRAN/"))
EOF
#之后可以随意安装了R包了，依赖问题解决
#把上面两句加入到~/.Rprofile，这样R会在启动时自动配置


Rscript packages.R
#这一段有些问题，等之后遇到再说

Rscript extrafont.R

rm .Rprofile

```



# rust

## bash $HOME/Scripts/dotfiles/rust/install.sh中内容：

```
#!/bin/bash

BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )

cd "${BASE_DIR}" || exit

#将目录挂载到$HOME/Scripts/dotfiles/rust/的目录，如果不成功则退出
#上面两步是为了将挂载目录，将下列安装 安装在$HOME/Scripts/dotfiles/rust/


mkdir -p $HOME/.cargo
#创建$HOME/.cargo目录

if grep -q -i RUST_PATH $HOME/.bashrc; then
#忽略大小写，看能否在$HOME/.bashrc目录匹配到RUST_PATH
    echo "==> .bashrc already contains RUST_PATH"
else
    echo "==> Updating .bashrc with RUST_PATH..."
    RUSTUP_DIST_SERVER="export RUSTUP_DIST_SERVER=https://ipv4.mirrors.ustc.edu.cn/rust-static"
    RUSTUP_UPDATE_ROOT="export RUSTUP_UPDATE_ROOT=https://ipv4.mirrors.ustc.edu.cn/rust-static/rustup"
    RUST_PATH="export PATH=\"\$HOME/.cargo/bin:\$PATH\""
    echo '# RUST_PATH'       >> $HOME/.bashrc
    echo $RUSTUP_DIST_SERVER >> $HOME/.bashrc
    echo $RUSTUP_UPDATE_ROOT >> $HOME/.bashrc
    echo $RUST_PATH          >> $HOME/.bashrc
    echo >> $HOME/.bashrc

    # make the above environment variables available for the rest of this script
    eval $RUSTUP_DIST_SERVER
    eval $RUSTUP_UPDATE_ROOT
    eval $RUST_PATH
    #eval执行内部嵌套的复杂语句
fi
#设置rust的各种环境变量并将其写入$HOME/.bashrc


echo "==> Install rustup"
curl https://sh.rustup.rs -sSf | bash -s -- -y
#curl -使用无声模式+即使使用s也打印错误+正常情况下，当HTTP服务器出现故障时交付一个文档时，它返回一个HTML文档，通常还描述了原因等
#如果-s选项存在，或者在选项处理之后没有参数存在，那么将从标准输入中读取命令。此选项允许在调用交互式shell时设置位置参数。
#一个--表示选项的结束，并禁止进一步的选项处理。在--之后的任何参数都被视为文件名和参数。参数-等价于--




rustup component add clippy rust-analysis rust-src rustfmt

#clippy - Lint your project using Clippy.   说明:检查代码规范，并给出优化方案（常用）命令： cargo clippy
#rustfmt - Format Rust code according to style guidelines.  说明：格式化rust代码（常用）



# This cargo mirror can't release crates
#tee $HOME/.cargo/config <<EOF
#[source.crates-io]
#registry = "https://github.com/rust-lang/crates.io-index"
#replace-with = 'ustc'
#[source.ustc]
#registry = "git://ipv4.mirrors.ustc.edu.cn/crates.io-index"
#
#EOF

cargo install cargo-expand
#Print the result of macro expansion and #[derive]expansion.
cargo install cargo-release

cargo install intspan
#cargo install 命令用于在本地安装和使用二进制 crate。它并不打算替换系统中的包；它意在作为一个方便 Rust 开发者们安装其他人已经在 crates.io 上共享的工具的手段。只有拥有二进制目标文件的包能够被安装。
```


# 补充说明：
1. 运行R文件
格式：
```
Rscript [--options] [-e expr] file [args] [> outfile]
* [-- options] ：
默认--slave --no-restore
* [- e expr] ：可以通过expr输入R的表达式。这个我不是很理解，想明白了再来补充
file ：输入脚本文件名
* [> outfile] ：输出文件名
```
2. 
```
BASE_DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )
cd "${BASE_DIR}" || exit
```
${BASH_SOURCE[0]}表示bash脚本的第一个参数（如果第一个参数是bash，表明这是要执行bash脚本，这时"${BASH_SOURCE[0]}"自动转换为第二个参数）  
例如
![9](./pictures/%E5%9B%BE%E7%89%879.png)
![10](./pictures/%E5%9B%BE%E7%89%8710.png)

