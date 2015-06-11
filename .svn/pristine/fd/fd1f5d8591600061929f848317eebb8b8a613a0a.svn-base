package org.apache.solr.analysis.cn;

import java.io.Reader;
import java.util.Map;

import org.apache.commons.lang.StringUtils;
import org.apache.lucene.analysis.Tokenizer;
import org.apache.lucene.analysis.util.TokenizerFactory;
import org.apache.lucene.util.AttributeFactory;

public class jiebaTokenizerFactory extends TokenizerFactory {
	
	private boolean needKeep = false;
	private boolean needBookMark = false;
	private boolean extractStar = false;
	private boolean model = false;
	
	public jiebaTokenizerFactory(Map<String, String> args) {
		super(args);
		this.needKeep = getBoolean(args, "needKeep", false);
		this.needBookMark = getBoolean(args, "needBookMark", false);
		this.extractStar = getBoolean(args, "extractStar", false);
		this.model = getBoolean(args, "model", false);
	}
	
	@Override
	public Tokenizer create(AttributeFactory factory, Reader input) {
		return new jiebaTokenizer(factory, input, needKeep,extractStar,needBookMark,model);
	}

}
