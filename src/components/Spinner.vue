<template>
  <div className="flex justify-center items-center h-screen">
    <div className="loadingSpinner w-12 h-12 rounded-full"></div>
  </div>
</template>
<script>
export default {
  name: "LoadingSpinner",
};
</script>
<style scoped>
@keyframes spinner {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.loadingSpinner {
  border: 10px solid #f3f3f3;
  border-top: 10px solid #383636;
  animation: spinner 1.5s linear infinite;
}
</style>
