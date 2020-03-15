# Team_Costco issue description

* **Title:** <br>
  [Glide RequestOptions cannot be backward compatible #4127.](https://github.com/bumptech/glide/issues/4127) 

* **Motivation:** <br>
  This issue describes a backward compatibility issue between Glide 4.8.0 and later versions. In Glide version 4.8.0, BaseRequestOptions class does not exist, which means when the code was compiled to class file, the relationship between RequestionOptions and BaseRequestOptions (parent-child) was not established. So when migrating from a higher to lower (i.e. 4.8.0) Glide version, the class file will not reference BaseRequestOptions and will not call the right methods.

* **Initial solution proposal:** <br>
  It should be considered as an enhancement of the latest version of Glide library. We could add some methods in the incompatible class reflecting methods from previous version of Glide while maintaining elegance and gracefulness of the code.