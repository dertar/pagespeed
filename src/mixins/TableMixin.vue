<script>
export default {
  props: {
    title: {
      required: true,
      type: String
    },
    subtitle: {
      required: false,
      type: String
    },
    endtitle: {
      required: false,
      type: String
    },
    data: {
      required: false,
      type: Array
    }
  }
};
</script>
