Run Gradle profiler on TC agent
===============================

Linux
-----

Use byobu
```bash
byobu
```

Choose the right JDK:
```bash
export JAVA_HOME=/opt/jdk/oracle-jdk-8-latest
```

Clone Gradle:
```
git clone https://github.com/gradle/gradle

```

You can now push changes from your local development environment by adding a remote:
```
git remote add dev16 dev16.gradle.org:gradle/.git
git push ubuntu28 some_branch
```

Checkout the branch on the agent and allow pushing to checked out branch:
```
cd gradle
git checkout some_branch
git config receive.denyCurrentBranch ignore
```

After re-pushing, just reset to the HEAD
```
git reset --hard HEAD
```

Build the Gradle branch you want to test:
```bash
git checkout some_branch
./gradlew install -Pgradle_installPath=~/gradle-install
```

Clone Gradle profiler and install it:
```bash
git clone https://github.com/gradle/gradle-profiler.git
cd gradle-profiler
./gradlew install
```

Add Gradle profiler to the path:
```bash
export PATH=~/gradle-profiler/build/install/gradle-profiler/bin:$PATH
```

```bas
gradle-profiler --scenario-file performance.scenarios --project-dir . upToDate --benchmark
```

Install JProfiler:
```bash
wget https://download-keycdn.ej-technologies.com/jprofiler/jprofiler_linux_10_0_4.tar.gz
tar -xzvf jprofiler_linux_10_0_4.tar.gz
export JPROFILER_HOME=~/wolfs-perf/jprofiler10.0.4/
```

Profile a build:
```bash
gradle-profiler --scenario-file performance.scenarios --project-dir . upToDate --profile jprofiler --jprofiler-home=${JPROFILER_HOME}
```
