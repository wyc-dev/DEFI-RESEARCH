<back></back>

<style>

back{
  position: fixed;
  width: 100%;
  height: 100vh;
  background: linear-gradient(-45deg, #FFFFFF68 0%, #000000cc 13%, #FFFFFF68 25%, #000000cc 38%, #FFFFFF68 50%, #000000cc 62%, #FFFFFF68 75%, #000000cc 87%, #FFFFFF68 100%);;
  background-size: 900% 900%;
  position: relative;
  animation: animate 31s ease-in-out infinite;
  display: flex;
  justify-content: center;
  align-items: center;
}


@keyframes animate{
  0%{
    background-position: 0 50%;
  }
  50%{
    background-position: 100% 50%;
  }
  100%{
    background-position: 0 50%;
  }
}
</style>