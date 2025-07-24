---
title: "COMPEST 16"
# date: 2019-04-18T15:34:30-04:00
categories:
  - blog
tags:
  - ctf
  - compest16
---

### Lets' Help John!
1. Pertama buka web Let’s Help John http://challenges.ctf.compfest.id:9016/ dan 
tampilannya akan seperti berikut:
![alt text](/_posts/image_compest/image.png)
2. Lalu klik play, dan akan menjadi seperti berikut:
   ![alt text](/_posts/image_compest/image-1.png)
3. Setelah diidentifikasi, ternyata web ini perlu referer http://state.com. Saya menggunakan curl, command dan hasilnya seperti ini di CMD
   ![alt text](/_posts/image_compest/image-2.png)
4. Saya menyadari bahwa responsenya menunjukan kalau (Make sure his Cookie quantity is not "Limited". Make it "Unlimited"!). Disini saya menambahkan 1 line command di CMD, dan inilah hasilnya. 
  ![alt text](/_posts/image_compest/image-3.png)
5. Setelah saya melihat responsenya, saya mendapat clue di (Change your User-Agent to "AgentYessir".). Lalu saya mengganti User-Agent saya menjadi AgentYessir seperti dibawah:
   ![alt text](/_posts/image_compest/image-4.png) 
6. Setelah melihat responsenya, saya mendapat clue (Great! To make it obvious for John, lets say it's From pinkus@cellmate.com.) saya menambah From line lagi di command curl saya seperti dibawah:
   ![alt text](/assets/images/obito.jpg) 
7. FLAG: `COMPFEST16{nOW_h3Lp_H1m_1n_john-O-jailmisc_8506972ce3}` 

You'll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
